# Propriétés de la classe Doc {#core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353}

La classe `Doc` dispose de plusieurs propriétés.

Ces propriétés peuvent être récupérées au moyen de la méthode
[`Doc::getPropertyValue`][docgetprop].

De façon générale, bien que ces propriétés ne soient pas marquées comme
*private*, leur affectation ne doit passer que par les setters appropriés. Les
propriétés n'ayant pas de setter spécifiques sont modifiées par effet de bord
d'autres méthodes du document (par exemple, `Doc::revision` ne doit pas être
modifié, mais [`Doc::revise`][docrevise] incrémente cette propriété).

adate
:   Date de dernier accès au document.

allocated
:   Identifiant système de l'utilisateur auquel le document est affecté.

archiveid
:   Id de l'archive dans laquelle est contenue ce document.
    
    Note : cette propriété n'est portée que par le document archivé.
    Pour trouver la liste des lignées documentaires archivées, il faut faire 
    une recherche sur toutes les révisions.

atags
:   Balises applicatives (Tags positionnés sur le document).
    
    Ces tags permettent aux applications de marquer les documents pour des 
    traitements spécifiques.
    
    Les tags sont stockés dans une chaîne de caractères et sont séparés par des 
    `\n`.
    
    La méthode `Doc::getATag()` vérifie la présence d'une balise applicative.

cdate
:   Date de création de la révision.
    
    Pour obtenir la date de création de la lignée documentaire, il faut
    récupérer celle de la révision 0.
    
    Le format de la date est `YYYY-MM-DD HH:MM:SS`.

classname
:   Nom de la classe associée au document. Utilisé uniquement pour les familles.

comment
:   Commentaire de révision.
    **Obsolète** (voir Objet DocHisto).

confidential
:   Indique si le document est confidentiel.
    
    Dans ce cas, *confidential* vaut 1.

cvid
:   Identifiant du document [contrôle de vue][CVDoc] associé à ce document.

doctype
:   Type de document.
    
    Utilisé en interne pour des optimisations de performance et des contrôles.
    
    *   *F* : Document normal (historiquement, F pour *Freedom Document*),
    *   *D* : Dossier (D pour *Dir*),
    *   *S* : Recherche (S pour *Search*),
    *   *P* : Profil,
    *   *T* : Temporaire,
    *   *I* : Incomplet,
    *   *Z* : Document supprimé (Z pour *Zombie*),
    *   *W* : Cycle de vie (W pour *Workflow*),
    *   *C* : Famille (C pour *Class*).

domainid
:   Voir la documentation de l'application offline.

dprofid
:   Identifiant du profil dynamique associé à ce document.
    
    Lorsque le document est soumis à un [profil dynamique][profil_dynamique], ce
    profil dynamique lui permet de calculer le profil qui lui sera appliqué,
    lequel est stocké sur le document lui-même. Ainsi, lorsque la propriété
    *dprofid* est renseignée, la propriété *profid* est égale à l'*id du
    document* (ce qui correspond à un profil dédié).

forumid
:   **Obsolète**

fromid
:   Id de la famille d'appartenance.

icon
:   Référence au fichier d'icone du document.
    
    La valeur est :
    
    *   le nom de l'icone
    *   ou la référence d'un fichier du vault 
        (sous la forme *&lt;type-mime>|&lt;vaultid>|&lt;file-title>*).

id
:   Identifiant unique du document.
    
    Il est calculé automatiquement par la base de données lors de l'ajout du
    document en base.

initid
:   Id du premier document de la lignée documentaire.
    
    Il est notamment utile pour les recherches.

ldapdn
:   Chemin LDAP dans le cas d'une copie sur un serveur LDAP.
    **obsolète**

lmodify
:   Permet de savoir si le document a été modifié depuis sa dernière révision.
    
    Les valeurs sont :
    
    *   `Y` : La révision est la révision courante et a été modifiée depuis la
        dernière révision ;
    *   `L` : La révision est la dernière révision avant la révision courante ;
    *   `D` : La révision est supprimée ;
    *   `N` :
        *   Toute révision figée du document (autre que la *N-1*) :
            dans ce cas, *locked* vaut -1 ;
        *   La révision est la révision courante du document, mais identique à
            la révision *N-1*.

lockdomainid
:   Voir la documentation de l'application offline.

locked
:   Indique l'identifiant système de l'utilisateur qui a verrouillé le document :
    
    *   Chiffre négatif (inférieur à -1) : La valeur absolue indique l'identifiant
        système de l'utilisateur ayant verrouillé le document automatiquement.
        
        Ce verrou est posé lorsqu'un utilisateur clique sur le bouton de
        modification d'un document, et est supprimé automatiquement lors de la
        fin de l'édition, à la fermeture du navigateur (au moyen de l'événement
        `onBeforeUnload`), ainsi que par le [script
        `cleanContext`][wsh_cleanContext].
        
    *   Chiffre positif : Identifiant système de l'utilisateur ayant verrouillé le
        document manuellement.
        
    *   `0` : Pas de verrou.
        
    *   `-1` : Document révisé (figé).

name
:   Nom logique du document.
    
    Référence toujours la version courante d'un document. Le nom logique est identique
    quelque soit la révision d'un même document. Lorsqu'on affecte un nom logique 
    il est affecté sur l'ensemble de la lignée documentaire.
    Si le nom logique est utilisé comme référence pour [accèder][new_doc] à un document, c'est la
    dernière révision qui sera retourné.

owner
:   Identifiant système de l'utilisateur ayant créé le document.
    
    Cette valeur est spécifique à chaque révision.

postitid
:   Identifiant du document post-it associé (famille *NOTE*).

prelid
:   Identifiant du document (dossier) de relation primaire.
    
    Cela correspond au parent lors d'une représentation arborescente.
    
    Cette propriété est gérée automatiquement avec les règles de gestion suivantes :
    
    *   Lorsque le document n'a jamais été inséré dans un dossier : 
        le premier dossier dans lequel est inséré le document ;
    *   Lorsque le document est déplacé dans un nouveau dossier : 
        le dernier dossier dans lequel est déplacé le document ;
    *   Lorsque le document est retiré d'un dossier : 
        si le document est dans un autre dossier, alors celui-ci est sélectionné.

profid
:   Identifiant du [profil de document][profdoc].
    
    *   Chiffre négatif : Profil non activé ;
    *   0 : pas de profil ;
    *   Vide : Pas de profil ;
    *   Chiffre positif : profil actif ;
        *   Identique à l'id du document : contrôle dédié ;
        *   Autre valeur positive : profil lié, non dynamique.

revdate
:   Date de révision.
    
    Pour un document non révisé, c'est la date de dernière modification.
    Le format de cette date un entier (_unix timestamp_).

revision
:   Numéro d'ordre du document dans sa lignée documentaire.

state
:   Étape du document ou référence à un identifiant de document *état libre*.

title
:   Titre du document.

usefor
:   Type d'utilisation du document :
    
    *   `S` : La famille est Système : elle n'apparaît pas par défaut dans la
        liste des familles pour les *recherches*, *recherches détaillées* ou
        *rapports*.
        
        Le caractère 'S' peut aussi être placé devant les autres caractères
        décrits ci-dessous pour masquer la famille dans les recherches.
        
    *   `W` : Le document est un cycle de vie (*W* pour *Workflow*) ;
        
    *   `G` : Le document est un intercalaire de chemise (*G* pour *Guide*) ;
        
    *   `P` : Le document est un profil (*P* pour *Profile*).

version
:   Libellé de la version : il est vide par défaut.
    
    La version est affichée à coté de l'état sur les documents et dans
    l'historique.
    
    *Note* : La version n'est pas obligatoirement numérique, elle peut aussi
    être alphanumérique ; mais elle ne doit pas contenir de retour de chariot.

wid
:   Identifiant du [document *cycle de vie*][WF_document] associé à ce document.

<!-- links -->
[docrevise]:        #core-ref:882e3730-0483-4dbc-9b9d-0d0b5cc31d38
[docgetprop]:       #core-ref:80e3ca49-26ea-4090-9fcd-7b92bf9d5c6d
[CVDoc]:            #core-ref:017f061a-7c12-42f8-aa9b-276cf706e7e0
[profil_dynamique]: #core-ref:bc24834a-b380-4681-ae94-08b93076a7e8
[wsh_cleanContext]: #core-ref:100b123b-da1a-45b4-848b-0622f3e09a40
[WF_document]:      #core-ref:b541e22f-5ece-4d19-8460-0cb0c5f3ec7a
[docprof]:          #core-ref:f1575705-10e8-4bf2-83b3-4c0b5bfb77cf
[docprofdyn]:       #core-ref:bc24834a-b380-4681-ae94-08b93076a7e8
[new_doc]:          #core-ref:e978cbd1-5f54-4a06-a6be-f1c079c2d734