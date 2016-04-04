# Écrire une application {#core-ref:395f44f1-6699-4ad8-b525-31e65e9b6efb}

Pour créer une nouvelle application, il faut au minimum un dossier et deux
fichiers :

    MYAPP
    |-- MYAPP.app
    `-- MYAPP_init.php

où *MYAPP* est le nom de l'application.

Si l'application est visible, l'icône de l'application doit être ajoutée
(image carrée entre 48 et 64 px) :

    Images/votre_image.png

Toutes les images utilisées par l'application doivent se trouver dans le
répertoire `Images/`.

L'application peut aussi contenir une ou plusieurs [actions][actions].

Tous les fichiers de template (XML/HTML/JS/CSS) doivent se trouver dans un
répertoire `Layout/`.

Tous les autres fichiers doivent se trouver à la racine du dossier.

Exemple : Arborescence d'une application avec deux actions :

    MYAPP
    |-- Images
    |   `-- MYAPP.png
    |-- Layout
    |   |-- myaction1.css
    |   |-- myaction1.xml
    |   |-- myaction2.js
    |   `-- myaction2.xml
    |-- myaction1.php
    |-- myaction2.php
    |-- MYAPP.app
    `-- MYAPP_init.php

Le dossier doit avoir le même nom que l'application et être mis à la racine du
[contexte][contexte].

## MYAPP.app {#core-ref:cf584c21-ebee-4444-8046-da3fa3a2db1b}

Ce fichier PHP décrit l'application.
Il contient au moins une variable de configuration :

`$app_desc` (`array`)
:   contient les [éléments de présentation de l'application][app_desc].

Il peut également contenir les variables suivantes :

`$action_desc` (`array`)
:   contient la définition des [actions de l'application][actions].

`$app_acl` (`array`)
:   contient la définition des [droits applicatifs (acls)][droits_applicatifs].

Exemple de contenu :

    [php]
    <?php
     $app_desc = array(
        "name"        => "MYAPP",
        "short_name"  => "Mon application",
        "description" => "Mon application de test",
        "icon"        => "votre_image.png",
        "displayable" => "Y",
        "childof"     => "ONEFAM"
     );
            

### Contenu possible pour `$app_desc` {#core-ref:f0dbbdd0-5f93-4173-be2f-bac715b80771}

Les différentes clés utilisables dans `$app_desc` sont :

**name** (obligatoire)
:   Nom système de l'application.
    
    C'est le nom qui apparaît dans les menus de configuration de Dynacase.
    
    Ce nom doit également correspondre au nom du dossier
    contenant l'application, ainsi qu'à celui des fichiers *.app* et *_init.php*
    (la comparaison est sensible à la casse).

**short_name** (facultatif)
:   Nom de l'application
    
    Il apparaît dans le menu déroulant de la barre d'application
    (si l'application est visible). Il n'est pas multiligne.

**description** (facultatif)
:   Nom qui apparaît en titre du sous-menu de l'application *APP_SWITCHER* 
    fourni par le module _dynacase-appswitcher_. Le sous-menu apparaît lors de
    la sélection de l'application dans le menu déroulant de la barre
    d'application (si l'application est visible). Il sert à décrire
    l'application et peut être multiligne.

**icon** (facultatif)
:   Nom du fichier image qui sera publié dans `MYAPP/Images/`.
    
    Ce fichier est
    l'icône qui répresente l'application MYAPP. Il doit se trouver dans le
    dossier `Images/` des sources.

**displayable** (facultatif)
: Un caractère pouvant avoir deux valeurs :
    
    *   `Y` : l'application apparaît dans le menu déroulant de la barre
        d'application du module _dynacase-appswitcher_.
    *   `N` : l'application sera invisible.  (par défaut)
    
    **Remarque** : Une application invisible peut quand même avoir ses actions
    appelées. Par exemple l'application "GENERIC" est invisible mais nombre de
    ses actions sont utilisées par l'application "ONEFAM".
    Ce paramètre est pris en compte par l'application *APP_SWITCHER* pour
    afficher ou non l'application dans les menus.  
    Cette valeur ne peut pas être changée lors des mises à jour. Elle ne peut
    être modifiée que via le _centre d'administration_.

**childof** (facultatif)
:   Indique si cette application [hérite d'une autre][childofapp].
    
    Il est possible de faire dériver une application de n'importe quelle autre
    application existante (ex: ONEFAM,GENERIC...).

   **Limitation** : Un seul niveau d'héritage est pris en compte.

**available** (facultatif) 
:   Indique si l'application est disponible. Deux valeurs possibles :
    
    * `Y` : application disponible (valeur par défaut) 
    * `N` : application non disponible. 
    
    Une application indisponible ne peut exécuter aucune de ses actions.
    Dans le cas d'une requête HTTP, le status "503 Application unavalaible"
    est renvoyé.
    
    Cette valeur ne peut pas être changée lors des mises à jour. Elle ne peut
    être modifiée que via le _centre d'administration_.

**tag** (facultatif)
:   Permet d'indiquer un tag. Les applications de Dynacase Core sont
    marquées "CORE". Ceci peut être utile pour réaliser des interfaces 
    qui donnent accès à plusieurs applications suivant leur tag. Pour 
    préciser plusieurs tags sur une même application, il faut les séparer par un
    espace. Exemple : Une application avec comme valeur dans tag `"CORE ADMIN"`
    a le tag `"CORE"` et le tag `"ADMIN"`.  
    Le tag "ADMIN" est réservé. Il signifie que l'application ne pourra pas être
    utilisée depuis le point d'entrée par défaut ("index.php") mais qu'à partir du
    _centre d'administration_ ("admin.php").

Ces clés correspondent aux propriétés de la [classe Application][classapplication]

## MYAPP_init.php {#core-ref:64c24cf9-b646-4a0f-9164-d0d146e12023}

Ce fichier PHP contient les paramètres déclarées par l'application
(globales, applicatives, utilisateurs). Il doit au moins contenir la version de
l'application.

Ces paramètres sont accessibles par l'administrateur, via le
_centre d'administration_.

Plus d'informations sur les [paramètres applicatifs][parametres_applicatifs].

## Initialisation ou mise à jour de l'application {#core-ref:cb0f90ab-63cb-4a4d-a19f-7d8c6990fc0e}

Pour initialiser l'application, il faut:

1.  Publier les sources dans un répertoire portant le même nom que l'application
    à la racine du contexte sur le serveur.
2.  Lancer le script `programs/record_application` qui se trouve sur le serveur.
    Ce script prend deux arguments: 
    *   une chaîne de caractère représentant le nom de l'application.
    *   la lettre `I` (pour l'installation) ou `U` (pour la mise à jour)
3.  Lancer le script `programs/update_catalog` pour mettre à jour les
    traductions.

Exemple d'initialisation d'application:

    [code bash]
    # Dans cet exemple,
    # Le répertoire courant est la racine du contexte sur le serveur,
    # l'utilisateur est celui du serveur web `apache`.
    $> cp -r /chemin_vers_mes_sources/MYAPP .
    $> ./programs/record_application MYAPP I
    $> ./programs/update_catalog

Pour la mise à jour, la suite de commandes est la même en remplaçant
`./programs/record_application MYAPP I` par
`./programs/record_application MYAPP U`.

**Remarque**: Une fois l'application initialisée, il est possible de faire la
mise à jour en passant par l'interface graphique du [centre d'administration]
[admin_center].


<!-- links -->
[actions]: #core-ref:e67d8aeb-939c-46e3-9be8-6fc3ba75ebc2
[contexte]: #core-ref:0081bf38-3c37-47e5-9c39-70579214abdd
[app_desc]: #core-ref:f0dbbdd0-5f93-4173-be2f-bac715b80771
[droits_applicatifs]: #core-ref:a98b72ea-c063-4907-abc4-e5171ab55e59
[parametres_applicatifs]: #core-ref:c3d9cb18-16d0-435a-b8c2-5fa6ac06c522
[childofapp]: #core-ref:3fb1bd33-0190-4e8c-96f5-6a8c0f084e6f
[classapplication]: #core-ref:5fca4352-702f-44fb-8ffa-3686545c6c67