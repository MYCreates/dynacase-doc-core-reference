# Application::getJSLink {#core-ref:95056b5d-5002-4a5d-bc26-31595fad9afd}

<div class="short-description">
Récupérer le lien de téléchargement (URL) d'un script JavaScript.
</div>

<!-- <div class="applicability"></div> -->

## Description {#core-ref:49ff5907-2fd3-4640-9a54-dad1a1130a20}


    [php]
    string getJSLink ( string $ref [, bool $needparse = false [, string $packName = '']] )

Permet de retourner le lien de téléchargement (URL) d'un script JavaScript
servie par Dynacase avec gestion de la version du cache (Voir [Notes](#core-
ref:3CD6C795-A11C-4128-B467-E221A81A0DF5)).

## Avertissements {#core-ref:79ada855-2c43-49ea-ab20-a35bf5f39deb}

N/A

## Liste des paramètres {#core-ref:0809adad-6f96-4867-bd11-0e83e6dd76ee}

(string) `ref`
:   La référence au script JavaScript.
    La référence peut être : un chemin d'accès relatif au contexte Dynacase (ex.
    `MY_APP/Layout/my_script.js`), une URL (ex.
    `http://www.example.net/my_script.js`), une action d'une application (ex.
    `MY_APP:JS`). La référence peut être : un chemin d'accès relatif au contexte
    Dynacase (ex. `MY_APP/Layout/my_script.js`, `my_script.js`), un chemin
    d'accès relatif au répertoire `Layout` du style du theme actuellement
    appliqué, un chemin d'accès relatif au répertoire `Layout` de l'application
    courante ou d'une application particulière (ex. `MY_APP:my_script.js`), une
    URL (ex. `http://www.example.net/my_script.js`).

(bool) `needparse`
:   Permet d'indiquer si le contenu du script doit être interprété comme un 
    template.

(string) `packName`
:   Les scripts peuvent être concaténées afin de réduire le nombre de fichiers
    chargés par le client. Dans ce cas, toutes les scripts ajoutées avec un même
    identifiant `packName` seront concaténées pour ne donner qu'un seul fichier
    à charger. Des `packName` différents peuvent être utilisés pour faire des
    groupes de scripts qui seront servis chacun dans un fichier unique et
    distinct par `packName`.

## Valeur de Retour {#core-ref:85258105-e450-41e9-a5e5-0470126ff1b4}

La méthode retourne une chaîne de caractère non vide avec le lien de
téléchargement du script JavaScript ajouté, ou une chaîne de caractère vide si
l'emplacement du script n'est pas valide.

## Erreurs / Exceptions {#core-ref:117b79ee-3a3b-420b-bf80-8c7392428369}

N/A

## Historique {#core-ref:781670bd-78ea-43f2-98c8-568840974615}

N/A

## Exemples {#core-ref:29e1119b-4f34-4071-97b7-c252401d58d5}

- Exemple #1

Script JavaScript `MY_APP/Layout/script.js` :


    [javascript]
    alert('Here I am!');

Contrôleur de l'action `MY_ACTION` (`my_action.php`) :


    [php]
    function my_action(Action &$action) {
        /* Get the Application of the current Action */
        $application = $action->parent;
        
        $jsLink = $application->getJSLink('MY_APP/Layout/script.js');
        
        $action->lay->set('JS_LINK', htmlspecialchars($jsLink));
    }

Vue de l'action `MY_ACTION` (`Layout/my_action.xml`) :


    [html]
    <!DOCTYPE html>
    <html>
      <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <title>My Action</title>
      </head>
      <body>
        <script type="text/javascript" language="JavaScript" src="[JS_LINK]"></script>
      </body>
    </html>

Résultat du rendu de la vue de l'action :


    [html]
    <!DOCTYPE html>
    <html>
      <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <title>My Action</title>
      </head>
      <body>
        <script type="text/javascript" language="JavaScript" src="MY_APP/Layout/script.js&wv=4253"></script>
      </body>
    </html>

## Notes {#core-ref:3cd6c795-a11c-4128-b467-e221a81a0df5}

- Les scripts ajoutées via cette méthode seront servis avec une directive 
indiquant au client de mettre en cache le résultat obtenu et minimiser ainsi les
requêtes au serveur.
- L'expiration de la mise en cache est gérée via un argument `wv=<version>` dont
le numéro de version est incrémenté à chaque mise à jour d'un module.
- Les `packName` sont composés par utilisateur et sont mis en cache sur le 
navigateur de l'utilisateur : ils ne sont donc pas partagés et/ou mis en cache
sur le serveur.

## Voir aussi {#core-ref:447d6155-3af5-4274-a308-cb647db1966a}

- [`Application::addJsCode`](#core-ref:49a8e28b-f286-45d7-b9e0-cc3591a8efde)
- [`Application::addJsRef`](#core-ref:b4b041aa-2649-498d-ace7-52131053c7db)

