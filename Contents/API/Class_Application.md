# Classe Application {#core-ref:5fca4352-702f-44fb-8ffa-3686545c6c67}

La classe *Application* gère les applications Dynacase. Cette classe gère
l'accès aux fonction globales liées à l'application comme les paramètres
applicatifs. Elle permet d'instancier les objets *Action* en vu de leur
exécution.


## Propriétés de la classe Application {#core-ref:e2510a47-76d3-4bb2-b03f-f773c3b8eff4}

Ces propriétés sont pour la plupart initialisées avec le contenu du fichier
".app" de description de l'application.

id
:   Identifiant numérique de l'application.
    Il est différent suivant les contextes car il est calculé en prenant le
    prochain numéro disponible lors de l'enregistrement de l'application.

name
:   Référence de l'application.
    Contient le nom donnée dans le fichier "*.app*" de description de
    l'application.

short_name
:   Description courte de l'application. 
    Issu du fichier "*.app*" de description de l'application.

description
:   Description longue de l'application. 
    Issu du fichier "*.app*" de description de l'application.

available
:   Indique la disponibilité de l'application.
    Elle contient soit `Y` soit `N`. `Y` indique qu'aucune action de
    l'application ne peut être exécutée. Si cette propriété vaut 'N', les
    actions ne pourront être exécutées et un message d'erreur est renvoyé à la
    place du résultat de l'action.

icon
:   Icone de l'application.

childof
:   Nom de l'application parente.
    Cela indique que les actions de l'application parente sont disponibles sur
    cette application.

tag
:   Permet d'indiquer une marque.
    Les applications de Dynacase Core sont marquées "CORE".


## Propriétés fonctionnelles {#core-ref:bfc9edaa-6814-423e-b654-d801a0185164}

Ces propriétés sont renseignées une fois que l'objet *Application* est
initialisé avec un identifiant valide.

user
:   Objet *Account* identifiant l'utilisateur courant

parent
:   Objet *Application* identifiant l'application parente.
    Généralement elle pointe vers l'application "CORE".

session
:   Objet *Session* identifiant la session de paramètres de l'utilisateur 
    courant.

