# Description des rôles {#core-ref:b9742040-0367-4a3d-a411-7195ec5fa7a4}

Les rôles représentent les fonctions des utilisateurs dans l'organisation. Ils
sont utilisés dans la définition des [droits][profilage].

## Caractéristiques des rôles {#core-ref:21a7f617-c64b-4d0d-9283-df10b129f6cd}

Les principaux attributs de la famille _Rôle_ sont ::

Nom `role_name`
:   Dénomination du rôle.

Référence `role_login` 
:   référence système.
    
    Il doit être unique sur l'ensemble des comptes (utilisateurs, rôles et
    groupes).
    
    **Note** : la référence doit toujours être en minuscules. Si elle comporte
    des majuscules elle seront transformées en minuscules lors de
    l'enregistrement.
    
    Depuis l'interface du _centre d'administration_, cette référence ne peut pas
    être renseignée, elle est calculée automatiquement avec un identifiant
    unique (par exemple : *role51949e892a469*).

Identifiant système `us_whatid`
:   Identifiant numérique système unique.
    
    Cet identifiant est donné par le système lors de l'enregistrement. Il sert
    d'identifiant pour l'objet système associé au document. Voir
    [Account][class_account].

## Création d'un rôle {#core-ref:e23ea348-c395-4472-9111-f90749fd5475}

Il est possible de créer manuellement un rôle en créant un document de la
famille `ROLE` depuis l'interface, à condition de bénéficier des privilèges
suffisants.

Par programmation il suffit de créer un document de la famille rôle `ROLE`.

    [php]
    $dr = \Dcp\DocManager::createDocument("ROLE");
    if ($dr) {
         $dr->setValue("role_login","designer");
         $dr->setValue("role_name","Concepteurs");
         
         $err=$dr->store();
         if (!$err) {
             print "nouveau rôle n°".$dr->getValue("us_whatid"); // affichage de l'identifiant numérique système
         } else {
             print "erreur:$err";
         }
     }
  

La méthode _ROLE::getAccount() permet de récupérer l'objet système "Account".
Voir le paragraphe [Account][class_account].

## Ajout d'un rôle à un utilisateur {#core-ref:2ed1d14c-b6a3-4ecd-9936-594e32fcd3b4}

L'ajout direct d'un groupe à un utilisateur se fait depuis le document
_Utilisateur_.  
C'est l'attribut `us_roles` qu'il faut modifier pour changer les associations de
rôles aux utilisateurs.

    [php]
    $dr = \Dcp\DocManager::getDocument(2435); // Rôle à ajouter
    $du = \Dcp\DocManager::getDocument(1073); // Utilisateur
    if ($du && $du->isAlive()) {
      $uRoles = $du->getMultipleRawValues("us_roles");
      printf("Ajout rôle %s pour %s\n",$dr->getTitle(),$du->getTitle());
      $uRoles[] = $dr->getProperty('initid');
      $uRoles = array_unique($uRoles); // éviter les doublons si déjà présent
      $err = $du->setValue("us_roles",$uRoles);
      
      if (!$err) {
        $err = $du->store();
      }
      
      if ($err) {
        print "Erreur: $err\n";
      } else {
        print "Nouvelle liste de rôles:\n";
        print_r($du->getAttributeValue("us_t_roles"));
      }
    }
    
## Association d'un rôle à un groupe {#core-ref:237c815c-0133-461a-8608-c5360f28233a}

L'association va propager le rôles à l'ensemble des membres du groupes et des
sous-groupes.  
C'est l'attribut `grp_roles` qu'il faut modifier pour changer les associations
de rôles aux groupes.

    [php]
    $dr = \Dcp\DocManager::getDocument(2435); // Rôle à ajouter
    $dg = \Dcp\DocManager::getDocument(2434); // Groupe
    if ($dg && $dg->isAlive()) {
        $gRoles = $dg->getMultipleRawValues("grp_roles");
        printf("Ajout rôle %s pour %s\n",$dr->getTitle(),$dg->getTitle());
        $gRoles[] = $dr->initid;
        $gRoles = array_unique($gRoles); // éviter les doublons si déjà présent
        $err=$dg->setValue("grp_roles",$gRoles);
        if (!$err) {
            $err = $dg->store();
        }
        if ($err) {
            print "Erreur: $err\n";
        } else {
            print "Nouvelle liste de rôles:\n";
            print_r($dg->getAttributeValue("grp_roles"));
        }
    }

<!-- links -->
[profilage]: #core-ref:ce576351-dbe6-45d1-8097-f9573502b651
[class_account]:  #core-ref:68c93fb2-088c-435a-b4ac-e1b94095d0c9