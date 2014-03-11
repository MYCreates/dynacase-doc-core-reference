# Classe Action {#core-ref:29553eba-bcea-4baf-bef8-103c3a3510fa}

La classe *Action* gère les actions des applications Dynacase. Cette classe
gère la sécurité, l'exécution et le rendu de l'action. Une action est toujours
liée à une application. L'accès web à une action se fait via une url du type :
`?app=APPLICATION_NAME&action=ACTION_NAME`.


## Caractéristiques de la classe Action {#core-ref:8b838f3f-1616-4df7-8826-7a6e43fdfd72}

Ces propriétés sont issues du fichier ".app" de description de l'application.
Leurs valeurs sont définies lors de l'enregistrement de l'application.

Ces caractéristiques ne doivent pas être modifiées par programmation sauf pour
des besoins précis d'administration des applications. La mise à jour de
l'application et par conséquent l'enregistrement du fichier ".app" écrase les
modifications qui auront pu être faites si les caractéristiques de l'action ont
été modifiées.

(int) id
:   Identifiant numérique de l'action.
    Il est différent suivant les contextes car il est calculé en prenant le
    prochain numéro disponible lors de l'enregistrement de l'action. Cette
    propriété ne doit pas être modifiée car elle est calculée par le système
    lors de l'enregistrement.

(int) id_application
:   Identifiant numérique de l'application portant l'action.
    Pour les mêmes raisons que celles de l'*id* celui-ci est différent suivant
    les contextes. Cette propriété ne doit pas être modifiée car elle est
    calculée par le système de part son appartenance à son application.

(string) name
:   Référence de l'action.
    Contient le nom donné dans le fichier "*.app*" de description de
    l'application.

(string) short_name
:   Description courte de l'action. Issu du fichier "*.app*" de description de 
    l'application.

(string) long_name
:   Description longue de l'action. Issu du fichier "*.app*" de description de 
    l'application.

(string) script
:   Nom du fichier PHP qui sera inclus lors de l'exécution de l'action.
    Il est issu du fichier "*.app*" de description de l'application si celui-ci
    est renseigné. Il est calculé s'il n'est pas renseigné. Dans ce dernier cas,
    le nom du script est le nom (*name*) de l'action en minuscule suivi de
    l'extension "*.php*". Le fichier PHP utilisé est doit être dans le
    répertoire de l'application (le nom du répertoire est le nom de
    l'application).

(string) function
:   Nom de la fonction PHP qui sera appelée lors de l'exécution de l'action.
    Il est issu du fichier "*.app*" de description de l'application si celui-ci
    est renseigné. Il est calculé s'il n'est pas renseigné. Dans ce dernier cas,
    le nom de la fonction est le nom (*name*) de l'action en minuscule.

(string) layout
:   Nom du fichier template utilisé lors de l'exécution de l'action.
    Il est issu du fichier "*.app*" de description de l'application si celui-ci
    est renseigné. Il est calculé s'il n'est pas renseigné. Dans ce dernier cas,
    le nom du template est le nom (*name*) de l'action en minuscule suivi de
    l'extension "*.xml*".  Le fichier *template* référencé doit être dans le
    sous-répertoire "*`Layout`*" du répertoire d'installation de l'application.

(string) available
:   Indique la disponibilité de l'action.
    Valeurs possibles :
    
    * `Y` : action disponible,
    * `N` : action non disponible.
    
    `Y` indique que cette action peut être exécutée. Si cette propriété vaut
    `N`, l'action ne pourra être exécutée et un message d'erreur sera renvoyé à
    la place du résultat de l'action.

(string) acl
:   Nom du droit nécessaire pour exécuter l'action.

(string) openaccess
:   Indique que l'action peut être exécutée avec un jeton d'authentification 
    sans demande de mot de passe.
    
    Valeurs possibles :
    
    * `Y` : action autorisée en mode open,
    * `N` : action non autorisée en mode open,
    * *vide* : idem `N`.
    
    L'accès avec jeton n'est autorisé que si la valeur est `Y`.

(string) root
:   Indique que cette action est l'action principale de l'application.
    
    Valeurs possibles :
    
    * `Y` : action principale,
    * `N` : action non principale.
    
    C'est celle qui sera lancée avec l'url "?app=*APPNAME*" sans préciser
    d'action. Elle contient soit `Y` soit `N`. L'action est déclarée principale
    si la valeur est `Y`. Une seule des actions de l'application doit être
    principale.

(string) icon
:   Icône de l'action. Non utilisé

## Principales propriétés de la classe Action {#core-ref:8bfc8d93-ec52-4049-8576-55b255f00e01}

Ces propriétés sont renseignées une fois que l'objet *Action* est initialisé
avec un identifiant valide. Elles sont accessibles depuis l'objet `Action` passé
en paramètre de la fonction PHP de l'action.

(Account) user
:   Objet `Account` identifiant l'utilisateur courant

(Application) parent
:   Objet `Application` identifiant l'application de l'action.

(Layout) lay
:   Objet `Layout` identifiant le template de représentation de l'action

(Session) session
:   Objet `Session` identifiant la session de paramètres de l'utilisateur courant.

(Log) log
:   Objet `Log` identifiant de l'objet `Log` permettant d'écrire des messages
    dans le système.
