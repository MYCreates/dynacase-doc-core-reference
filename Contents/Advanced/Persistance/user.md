# Manipulation des comptes utilisateur {#core-ref:68c93fb2-088c-435a-b4ac-e1b94095d0c9}

Ce chapitre a pour but de montrer, par l'utilisation des méthodes et fonctions
les plus courantes, la manipulation des utilisateurs "interne".

Ce chapitre ne décrit pas la manipulation des utilisateurs réseaux disponibles
avec le module "dynacase-networkuser".

## Création d'un utilisateur {#core-ref:5c7928ef-d778-4530-968d-4084b3f70c16}

La création d'un utilisateur peut être fait à l'aide de la [famille
`IUSER`][users].

Les comptes utilisateurs sont gérés par la classe `Account`. La famille `IUSER`
permet de faire le lien entre le compte "système" (classe `Account`) et le
document (famille `IUSER`).

Un utilisateur est identifié par un numéro unique. Ce numéro est renseigné par
l'[attribut `us_whatid`][dbuser] du document `IUSER`. Il correspond à l'attribut
"id" de la classe `Account`.

Code minimaliste pour créer un utilisateur via les documents.

    [php]
    include_once("FDL/Class.Doc.php");
    $iUser=\Dcp\DocManager::createDocument('IUSER');
    if ($iUser) {
        $iUser->setValue("us_login","jean.martin");
        $iUser->setValue("us_lname","martin");
        $iUser->setValue("us_fname","jean");
        $iUser->setValue("us_passwd1","secret");
        $iUser->setValue("us_passwd2","secret"); // nécessaire de doubler à cause des contraintes
        $err=$iUser->store();
        if (!$err) {
          print "nouvel utilisateur n°".$iUser->getValue("us_whatid"); // affichage de l'identifiant système
        } else {
          print "\nerreur:$err";
        }
    }


## Correspondance entre compte User et document IUSER {#core-ref:4842d6c2-f88d-41d9-aa30-04437ffdb67e}

Pour récupérer le compte système à partir du document `IUSER` :

    [php]
    $user=$userDoc->getAccount();
    if ($user->id != $doc->getValue("us_whatid") ) {
        // ce n'est pas normal
    }

Si vous ne disposez que de l'identifiant documentaire vous pouvez aussi utiliser
la méthode setFid de la classe Account:

    [php]
    $user=new Account();
    $user->setFid($docUserId);
    if ($user->isAffected()) {
       print $user->login;
    }

A l'inverse pour récupérer l'objet documentaire à partir du compte système, vous
pouvez utiliser l'attribut "fid".

    [php]
    $user=new Account();
    $user->setLoginName("john.doe");
    if ($user->isAffected()) {
      $doc=\Dcp\DocManager::getDocument( $user->fid);
    }

Attributs de correspondance : Compte User &hArr; Document IUSER

| Compte User | Document `IUSER` |
| ----------- | ---------------- |
| id          | us_whatid        |
| login       | us_login         |
| mail        | us_mail          |
| firstname   | us_fname         |
| lastname    | us_lname         |
| fid         | id               |


Exemple de création d'utilisateur via la classe `Account`

    [php]
    $user=new Account();
    $user->login='jean.dupond';
    $user->firstname='Jean';
    $user->lastname='Dupond';
    $user->password_new='secret';
    $err=$user->add();
    if ($err) {
      error_log($err);
    } else {
      print "Nouvel utilisateur n°".$user->id;
    }

## Groupe d'utilisateurs  {#core-ref:6afad021-29c2-400b-87f2-3a83551e3e95}

La famille `IGROUP` permet de rajouter des utilisateurs dans des groupes.

La famille `IGROUP` comme la famille `IUSER` utilise la classe `Account` pour
identifier les groupes "système".

Comme la famille `IUSER`, la famille `IGROUP` dispose de la méthode
`IGROUP::getAccount()` pour récupérer l'objet `Account` correspondant

### Correspondance Compte Groupe &hArr; Document IGROUP {#core-ref:70bec060-4ebe-48d5-a8d5-a8d7f500662a}


| Compte Groupe | Document `IGROUP` |
| ------------- | ----------------- |
| id            | us_whatid         |
| login         | us_login          |
| lastname      | grp_name          |
| fid           | id                |


Le compte "Account" d'un groupe a toujours son attribut "accounttype" à "G".

### Ajout d'un utilisateur à un groupe {#core-ref:dc529ed1-63eb-4628-aa62-3e3e213a09e4}

Utilisation de `Dcp\Family\IGROUP::insertDocument()`.

Ajout de l'utilisateur n°1009 dans le groupe GDEFAULT :

    [php]
    include_once("FDL/Class.Doc.php");
     
    $dbaccess = getDbAccess();
     
    $group = \Dcp\DocManager::getDocument("GDEFAULT");
    $user = \Dcp\DocManager::getDocument(1009); // 1009 est la référence documentaire de l'utilisateur
     
    printf("ajout de l'utilisateur %s [%d] au groupe %s [%d]\n",
           $user->getTitle(),$user->id,$group->getTitle(),$group->id);
     
    printf("liste des groupes avant\n");
    print_r($user->getTValue("us_idgroup"));
     
    $err=$group->insertDocument($user->initid);
    print "Error:$err\n";
     
    printf("liste des groupes apres\n");
    print_r($user->getTValue("us_idgroup"));

### Suppression d'utilisateur dans un groupe {#core-ref:c6c94659-4e24-49f8-9644-cd240a68f85c}

Utilisation de `Dcp\Family\IGROUP::removeDocument()`.

    [php]
    include_once("FDL/Class.Doc.php");
     
    $dbaccess=getDbAccess();
     
    $group=\Dcp\DocManager::getDocument("GDEFAULT");
    $user=\Dcp\DocManager::getDocument(1009);
     
    printf("suppression de l'utilisateur %s [%d] du groupe %s [%d]\n",
           $user->getTitle(),$user->id,$group->getTitle(),$group->id);
    
    printf("liste des groupes avant\n");
    print_r($user->getTValue("us_idgroup"));
     
    $err=$group->removeDocument($user->initid);
    print "Error:$err\n";
     
    printf("liste des groupes apres\n");
    print_r($user->getTValue("us_idgroup"));

### Récupération des membres d'un groupe {#core-ref:81cf3f6f-d7fd-439c-808e-c74bd17ad429}

À partir de l'objet "Account" d'un groupe : `Account::getAllMembers()`.

Exemple d'utilisation :

    [php]
    $docGroup=\Dcp\DocManager::getDocument("GDEFAULT");
    $group=$docGroup->getAccount();
    $members=$group->getAllMembers();
    $userDocIdList=array();
    foreach ($members as $user) {
        printf("%s) %s\n", $user["id"],$user["login"]);
        $userDocIdList[]=$user["fid"];
    }
    print "---\n";
    // recherche des documents `IUSER` correspondants
    $documentList=new DocumentList();
    $documentList->addDocumentIdentificators($userDocIdList);
    foreach ($documentList as $docid=>$docIUser) {
        printf("%s)%s\n", $docIUser->id,  $docIUser->getTitle());
    }

## Rôles

La famille `ROLE` permet d'associer des rôles à des utilisateurs ou à des
groupes.

La famille `ROLE` comme la famille `IUSER` utilise la classe `Account` pour
identifier les groupes "système".

Comme la famille `IUSER`, la famille `ROLE` dispose de la méthode
`ROLE::getAccount()` pour récupérer l'objet `Account` correspondant

### Correspondance Compte Rôle &hArr; Document ROLE 

| Compte Rôle | Document `ROLE` |
| ----------- | --------------- |
| id          | us_whatid       |
| login       | role_login      |
| lastname    | role_name       |
| fid         | id              |

## Récupération des utilisateurs associés à un rôle {#core-ref:30ae82d0-6ac6-4d50-8d23-af1d3c64d969}

Comme pour les groupes, à partir de l'objet "Account" d'un rôle :
`Account::getAllMember()`.

Exemple d'utilisation :

    [php]
    $docRole=\Dcp\DocManager::getDocument( "TST_ROLE");
    $members=$docRole->getAccount()->getAllMembers();
    $userDocIdList=array();
    foreach ($members as $user) {
      printf("%s) %s\n", $user["id"],$user["login"]);
      $userDocIdList[]=$user["fid"];
    }
    print "---\n";
    // recherche des documents `IUSER` correspondants
    $documentList=new DocumentList();
    $documentList->addDocumentIdentificators($userDocIdList);
    foreach ($documentList as $docid=>$docIUser) {
      printf("%s)%s\n", $docIUser->id,  $docIUser->getTitle());
    }

## Récupération des rôles d'un utilisateur {#core-ref:d012d0f0-79fb-4c09-97aa-2aa9da750e62}

À partir de l'objet "Account" , méthode `Account::getAllRoles()`

Exemple d'utilisation :

    [php]
    $user=new Account('');
    $user->setLoginName('john.doe');
    if ($user->isAffected()) {
      $roles=$user->getAllRoles();
      foreach ($roles as $aRole) {
        printf("%d)%s\n", $aRole["id"],$aRole["login"]); 
      }
    }

## Suppléants et titulaires {#core-ref:8bda02a2-0368-4f0d-aee6-68be7a2c604d}

Affectation d'un suppléant à un utilisateur et récupérations des titulaires.

    [php]
    $user=new Account(); 
    $user->setLoginName("j1"); 
    if ($user->isAffected()) { 
      $err=$user->setSubstitute("j2"); // J1 est le titulaire de J2 
                                    //(J2 est suppléant de J1) 
    } 
    $user->setLoginName("j3"); 
    if ($user->isAffected()) { 
      $err=$user->setSubstitute("j2");// J3 est le titulaire de J2 
    } 
    $user->setLoginName("j2"); 
    $incumbents=$user->getIncumbents(); // J2 a comme titulaire J1 et J3 
    foreach ($incumbents as $key=> $aIncumbent) { 
      printf("%d)%s\n", $key,Account::getDisplayName($aIncumbent)); 
    }

Résultat :

    www-data@luke:~$ ./wsh.php --api=testSubstitute
    0)j1 Doe 
    1)j3 Doe 

## Recherche de comptes {#core-ref:77224212-a4f3-4159-8a8a-c11c6f430f61}

Il est possible de rechercher les comptes suivant leurs critères d'appartenance
à des rôles ou des groupes. La classe [`SearchAccount`][searchaccount] permet de
réaliser facilement la recherche de compte. Le résultat de cette recherche peut
retourner des utilisateurs, des groupes ou des rôles.

### Recherche des utilisateurs par rôle {#core-ref:b48372db-c2a9-481a-a502-174f972484a3}

Pour indiquer le filtre d'un rôle, il faut utiliser, la méthode
`::addRoleFilter()`. Cette méthode prend en argument la référence du rôle. La
référence correspond à l'attribut 'role_login' du document ou à l'attribut login
de l'objet Account. Il ne correspond pas au nom logique du document.

    [php]
    $searchAccount = new SearchAccount();
    $searchAccount->addRoleFilter('writter');
    $searchAccount->setObjectReturn($searchAccount::returnAccount);
    /**
     * @var \AccountList $accountList
     */
    $accountList = $searchAccount->search();
    /**
     * @var \Account $account
     */
    foreach ($accountList as $account) {
      $login = $account->login;
      print "$login\n";
    }
    
Pour rechercher à partir du nom logique du document rôle, il faut utiliser la
méthode ::docName2login de correspondance fournie par la classe.

    [php]
    $searchAccount->addRoleFilter($searchAccount->docName2login('TST_ROLEWRITTER'));

La méthode `SearchAccount::setObjectReturn()` permet d'indiquer le type de
résultat obtenu par la méthode `SearchAccount::search()`.  Par défaut cela
retourne un objet AccountList qui est un itérateur sur des objets Account. Il
est possible d'indiquer `SearchAccount::returnDocument`, pour que la méthode
`SearchAccount::search()` retourne un objet [`DocumentList`][doclist] qui
donnera les documents correspondants

    [php]
    $searchAccount = new SearchAccount();
    $searchAccount->addRoleFilter($searchAccount->docName2login('TST_ROLEWRITTER'));
    $searchAccount->setObjectReturn($searchAccount::returnDocument);
    /**
     * @var \DocumentList $documentList
     */
    $documentList = $searchAccount->search();
    /**
     * @var \Doc $doc
     */
    foreach ($documentList as $doc) {
      $login = $doc->getValue("us_login");
      print "$login\n";
    }

Si on précise plusieurs rôles séparés par un espace, cela indiquera une
disjonction (OU).

    [php]
    $searchAccount->addRoleFilter("writter controller");

indique que les comptes recherchés ont le rôle "writter" ou "controller". 

Cette écriture est équivalente à :

    [php]
    $searchAccounts->addRoleFilter("writter");
    $searchAccount->addRoleFilter("controller");

La condition d'appartenance à plusieurs rôles n'est pas disponible avec cette
méthode. Ce filtre peut retourner des groupes ou des utilisateurs.

### Recherche des utilisateurs par groupe {#core-ref:bb14d2a0-abf6-470e-b226-186f15bc7784}

La méthode `::addGroupFilter()` est équivalent à `::addRoleFilter()`. Elle permet de
rechercher parmi les comptes qui appartiennent à différents groupes. Si cette
méthode est combinée à la méthode :`:addRoleFilter()` cela indique tous les
comptes qui appartiennent à un des groupes cités ou à un des rôles cités.

    [php]
    $searchAccounts->addGroupFilter("all"); // tous les utilisateurs du groupe "all"

La recherche par groupe recherche aussi les comptes dans les sous-groupes. Ce
filtre peut retourner des groupes ou des utilisateurs.

### Recherche sur des critères de compte {#core-ref:2bc3eab4-9779-4603-8a0a-b298a905f46b}

La méthode ::addFilter() permet d'ajouter des filtres sur les caractéristiques
des comptes:

*   login
*   firstname
*   lastname
*   mail
*   accounttype
*   status

&nbsp;

    [php]
    $mailExpr='test';
    $searchAccount = new SearchAccount();
    $searchAccount->addFilter("mail ~ '%s'", $mailExpr); // filtre les mail qui contiennent test
    $accountList = $searchAccount->search();
    foreach ($accountList as $account) {
      printf("%s => %s\n ", $account->login, $account->mail);
    }

La méthode `SearchAccount::addFilter()` ajoute une condition supplémentaire sur
le filtre. Le premier argument est la partie statique du filtre et les suivants
sont les arguments du filtre comme pour sprintf.

Au contraire de `SearchAccount::addGroupFilter()` ou
`SearchAccount::addRoleFilter()` les filtres sont autant de critères ajoutés à la
condition finale.

La méthode `SearchAccount::setTypeReturn()` permet de préciser le type de compte
à retourner : Utilisateur, Groupe ou Rôle.

    [php]
    $searchAccount->setTypeFilter($searchAccount::userType) ; // limité aux utilisateurs
    $searchAccount->setTypeFilter($searchAccount::userType | $searchAccount::groupType ) ;// limité aux utilisateurs et aux groupes
    $searchAccount->setTypeFilter($searchAccount::roleType) ; // limité aux rôles

Le paramètre est une combinaison des 3 constantes userType, groupType et
roleType.

Par défaut, les comptes retournés ne sont pas liés aux privilèges du document
associé. Si on désire ne rechercher que parmi les comptes que l'utilisateur
courant a le droit de voir il faut rajouter l'appel à la méthode
`SearchAccount::useViewControl()` avant l'appel à `SearchAccount::search()`.

    [php]
    $searchAccount->useViewControl(true);

## Spécialisation des documents utilisateurs {#core-ref:6ec2016f-4410-4c34-8b87-36a62859577e}

Les documents système "utilisateur" (famille `IUSER`) peuvent être dérivés pour
ajouter des informations fonctionnelles ou pour ajouter des informations
techniques dans le cas d'un backend d'[authentification spécifique][backend]. 

La famille `IUSER` n'impose pas de contrainte de syntaxe sur le login. La seule
contrainte est que ce "login" doit être unique parmi tous les comptes
(utilisateurs, groupes et rôles). Si vous voulez ajouter une contrainte de
syntaxe sur votre famille "utilisateur" dérivé de `IUSER` vous devez surcharger
la méthode `IUSER::constraintLogin()` qui est une méthode contrainte.

Exemple :

    [php]
     // constraint to limit login to 6 characters
    public function constraintLogin($login)
     {
            //only six alpha num
            $err = '';
            if (!preg_match("/^[a-z0-9]{6}$/i", $login)) {
                $err = _("the login must have six characters");
            }
            // must verify unicity also
            if (!$err) return parent::constraintLogin($login);
            return $err;
     }
 
Attention: lorsque la famille `IUSER` est dérivée, il faut reprendre et adapter
le paramétrage en terme de sécurité (profil, contrôle de vue) en fonction de vos
besoins.

<!-- links -->
[users]:        #core-ref:2bd98eec-5b03-4af0-b9d6-1bbf78fe9733 "Utilisateurs, groupes et rôles"
[dbuser]:       #core-ref:6d5684f4-73e8-431c-8b2b-6224a9e6b074 "Table users"
[backend]:      #core-ref:5a149c1c-c262-4aa6-b7b8-b66135140c49 "Provider d'authentification"
[doclist]:      #core-ref:23c71c28-dbce-4d34-819a-50d5bc4a38c3 "Classe DocumentList"
[searchaccount]:#core-ref:dc8236f9-2389-49a6-b582-bc6b332880b1