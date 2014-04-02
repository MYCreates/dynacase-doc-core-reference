# Description des utilisateurs {#core-ref:02f32b3d-be94-44f7-9b98-879c6b551c4a}

## Principales caractéristiques des utilisateurs {#core-ref:f69992c4-9588-47b9-b7a6-b3519b59cb25}

Les principaux attributs de la famille _Utilisateur_ sont :

Nom `us_lname` 
:   Nom de famille de l'utilisateur

Prénom `us_fname`
:   Prénom de l'utilisateur

Mail principal `us_mail`
:   Adresse de courriel de l'utilisateur

Login `us_login`
:   Identifiant de connexion. Cet identifiant doit être unique pour l'ensemble
    des comptes; unique pour les utilisateurs mais aussi pour les références de
    groupe et de rôle.
    
    **Note** : Le login doit toujours être en minuscules. S'il comporte des
    majuscules elle seront transformées en minuscules lors de l'enregistrement.  
    Cependant, depuis l'interface de connexion, la vérification du login est
    insensible à la casse.

Rôles `us_roles`
:   Liste des rôles associés à l'utilisateur. Ces rôles peuvent être affectés
    directement sur l'utilisateur ou obtenus en fonction de ses groupes
    d'appartenance.

Identifiant système `us_whatid`
:   Identifiant numérique système unique.
    
    Cet identifiant est donné par le système lors de l'enregistrement. Il sert
    d'identifiant pour l'objet système associé au document. Voir
    [Account][class_account].

## Mot de passe {#core-ref:2f070320-45f3-4664-9a80-d86634bb21a3}

Le mot de passe exigé par défaut n'a pas de contrainte. Cependant les paramètres
applicatifs suivant permettent d'imposer une force minimum au mot de passe :

`AUTHENT_PWDMINLENGTH`
:   Nombre minimum de caractères d'un mot de passe

`AUTHENT_PWDMINDIGITLENGTH`
:   Nombre minimum de chiffre d'un mot de passe

`AUTHENT_PWDMINUPPERALPHALENGTH`
:   Nombre minimum de lettres majuscule d'un mot de passe

`AUTHENT_PWDMINLOWERALPHALENGTH`
:   Nombre minimum de lettres minuscules d'un mot de passe

`AUTHENT_PWDMINSYMBOLLENGTH`
:   Nombre minimum de caractères qui ne sont ni des lettres ni des chiffres

Le mot de passe utilisé pour les comptes internes peut également contenir des
lettres accentuées.

Le mot de passe n'est pas stocké en clair dans la base. Seul son empreinte
(SHA256) est stocké afin de vérifier le mot de passe lors de la connexion.

## Validation du compte utilisateur {#core-ref:52feb7fc-dcba-44f0-8229-bc80c203b5d3}

### Date d'expiration des comptes {#core-ref:3c68911e-bee2-40b2-9f28-6c0ec68e4077}

Il est possible d'indiquer une date d'expiration du compte sur le *document
utilisateur*. Lorsque cette date est atteinte, toute connexion avec ce compte
est refusée et un message indique à l'utilisateur que le compte a expiré.

Cette date est contenue dans l'attribut `us_accexpiredate` de la fiche
utilisateur.

Il est également possible de définir une durée de validité par défaut au moyen
du paramètre applicatif `AUTHENT_ACCOUNTEXPIREDELAY` de l'application `AUTHENT`
(*Délai par défaut avant expiration du compte (jours)*).  
Ce délai exprimé en jour, est utilisé lors de la création d'un compte afin de
calculer la date d'expiration de ce compte.

La date d'expiration est contrôlée quelque soit le mode d'authentification
(Dynacase, AD/LDAP, SSO, …).
Si elle n'est pas renseignée, le compte n'expire pas.

Ce contrôle n'est pas réalisé pour le super administrateur Dynacase (admin).

### Limitation des tentatives de connexion {#core-ref:3c0e862a-fa7b-44d4-b85a-64b22754ce88}

Il est possible d'interdire la connexion pour un compte suite à un trop grand
nombre de mots de passe incorrects.

Ce nombre maximal de tentatives est défini au moyen du paramètre
`AUTHENT_FAILURECOUNT` de l'application `AUTHENT` (*Nombre d'échecs
d'authentification avant la désactivation du compte*).

Lors d'une demande d'authentification :

*   si le compte est activé
    *   si le mot de passe est valide
        *   le compteur est remis à 0
    *   si le mot de passe est erroné
        *   un compteur est incrémenté
        *   si ce compteur dépasse la valeur du paramètre
            `AUTHENT_FAILURECOUNT`, le compte est désactivé
*   si le compte est désactivé
    *   si le mot de passe est erroné
        *   un compteur est incrémenté
    *   dans tous les cas, un message est retourné

Lors de l'installation de dynacase, le paramètre `AUTHENT_FAILURECOUNT` est
positionné à `0`. La valeur 0 indique que le contrôle n'est pas réalisé.

Le compteur est disponible sur la fiche utilisateur au moyen de l'attribut
`us_loginfailure`. Un menu permet de réinitialiser cette valeur
(<span class="menu"><span class="menuitem">Compte</span>
<span class="menuitem">Réinitialiser échecs de connexion</span></span>)

Le contrôle est réalisé quelque soit le mode d'authentification
(Dynacase, AD/LDAP, SSO, …).

Ce contrôle n'est pas réalisé pour le super administrateur Dynacase (admin).

### Activation / désactivation du compte à la demande {#core-ref:88e94d8d-d5c6-4bc1-a7c4-f2a7424a9a5a}

Il est possible d'activer ou de désactiver un compte utilisateur à la demande
(par défaut, un compte est actif à sa création).

La fiche utilisateur présente les menus <span class="menu">
<span class="menuitem">Compte</span>
<span class="menuitem">Désactiver le compte</span></span> et <span class="menu">
<span class="menuitem">Compte</span>
<span class="menuitem">Réactiver le compte</span></span>.

L'activation / désactivation de compte n'influe pas sur les droits applicatifs
ou documentaires liés aux comptes.

Un compte utilisateur désactivé ne peut plus être utilisé pour se connecter.

Le contrôle est réalisé quelque soit le mode d'authentification
(Dynacase, AD/LDAP, SSO, …).

Ce contrôle n'est pas réalisé pour le super administrateur Dynacase (admin).

## Droit d'accès  {#core-ref:0257b73d-1e77-45a1-8412-39c2c132cb76}

par défaut, les documents de comptes sont accessibles à tous les utilisateurs,
mais leur modification est réservée aux administrateurs (utilisateurs
appartenant au groupe `administration` - identifiant `GADMIN`).

Pour le document `utilisateur`, le détenteur du compte peut modifier ses *nom*,
*prénom*, *mail* et *mot de passe*.

## Création d'un utilisateur {#core-ref:2f8209b9-41ab-4ae8-846c-ab2b1a80a597}

Il est possible de créer manuellement un utilisateur en créant un document de la
famille `IUSER` depuis l'interface, à condition de bénéficier des privilèges
suffisants.

Par programmation il suffit de créer un document de la famille `IUSER`.

    [php]
    $du = \Dcp\DocManager::createDocument("IUSER");
    if ($du) {
        $du->setValue("us_login","jean.martin");
        $du->setValue("us_lname","martin");
        $du->setValue("us_fname","jean");
        $du->setValue("us_passwd1","secret");
        $du->setValue("us_passwd2","secret"); // nécessaire de doubler à cause des contraintes
        
        $err = $du->store();
        
        if (!$err) {
            print "nouvel utilisateur n°".$du->getValue("us_whatid"); // affichage de l'identifiant numérique système
        } else {
            print "erreur : $err";
        }
    }

La méthode _IUSER::getAccount() permet de récupérer l'objet système "Account".
Voir le paragraphe [Account][class_account].


## Titulaires et suppléants {#core-ref:1591eb1c-aead-4f7b-bde9-5f42e397b22e}

Un utilisateur peut désigner un suppléant. Le suppléant reçoit tous les
privilèges de celui qui l'a désigné. Ce dernier est nommé le titulaire.

*   Un suppléant peut
    *   voir, modifier tous les documents que le titulaire peut voir et
        modifier,
    *   accéder à toutes les applications auxquelles le titulaire a accès.
*   Un suppléant ne peut pas
    *   modifier le document *utilisateur* de ses titulaires. Ainsi le mot de
        passe du titulaire ne peut pas être changé par son suppléant.

Un titulaire ne peut désigner qu'un seul suppléant.
Par contre, un suppléant peut avoir plusieurs titulaires.

Le suppléant est ajouté aux destinataires des courriels qui ont été lancés à
partir d'un modèle de mail à destination du titulaire (Le texte « suppléant »
est ajouté dans le nom des adresses email des suppléants afin de les
différencier des titulaires).

Lors d'une création ou d'une modification d'un document par un suppléant ou lors
d'un changement d'état, le texte de l'historique indique que cela est fait par
un suppléant dans le cas où celui-ci n'a pas directement (c'est à dire sans
qu'il soit suppléant) le droit de le faire.

Les droits sur les suppléants ne sont pas propagés. Ainsi :

*   Si l'utilisateur C est suppléant de B
*   et l'utilisateur B est suppléant de A

alors

*   l'utilisateur C a seulement les privilèges d'origine de B en plus des siens,
*   l'utilisateur B a les privilèges d'origine de A en plus des siens.

L'utilisateur C n'a pas les privilèges de A.

La modification d'un suppléant par l'interface est soumis au droit `ESUBSTITUTE`
du contrôle de vue *confidentiel utilisateur* qui est associé à la famille
`IUSER`.  
Par défaut aucun utilisateur n'a la droit de désigner un suppléant.

## Dérivation de la famille utilisateur {#core-ref:86344b1f-d6ab-4fb4-b676-bea2acbf1a00}

La famille utilisateur `IUSER` peut être dérivée comme les autres familles.

Cependant, la famille `IUSER` déclarant des hameçons sur le _postStore_,
_postCreate_ et _postDelete_, il faut bien faire attention à rappeler le parent
en cas de surcharge des hameçons.

De même, un contrôle de vues est attaché par défaut aux utilisateurs, il faut
aussi reprendre ses vues si la famille dérivée nécessite un autre contrôle de
vue.

<!-- links -->
[class_account]:  #core-ref:68c93fb2-088c-435a-b4ac-e1b94095d0c9
