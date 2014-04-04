# Application::addJsRef {#core-ref:b4b041aa-2649-498d-ace7-52131053c7db}

<div class="short-description">
Ajouter un lien vers un script Javascript à un template.
</div>

<!-- <div class="applicability"></div> -->

## Description {#core-ref:b72c0289-774f-4bb9-8bc6-25c89216bf87}


    [php]
    string addJsRef ( string $ref [, bool $needparse = false [, string packName = '']] )

Permet d'ajouter un lien vers un script JavaScript qui sera inséré dans les
éléments `[JS:REF]` des templates utilisées lors du rendu d'une vue.

## Avertissements {#core-ref:77ba2e0e-2d54-4c7f-9f7b-3286c1fc9883}

N/A

## Liste des paramètres {#core-ref:52e58f18-54fd-41ec-99b7-eb736dac7ba2}

(string) `ref`
:   La référence au script JavaScript.
    La référence peut être : un chemin d'accès relatif au contexte Dynacase (ex.
    `MY_APP/Layout/my_script.js`, `my_script.js`), un chemin d'accès relatif au
    répertoire `Layout` du style actuellement appliqué, un chemin d'accès
    relatif au répertoire `Layout` de l'application courante ou d'une
    application particulière (ex. `MY_APP:my_script.js`), une URL (ex.
    `http://www.example.net/my_script.js`).

(bool) `needparse`
:   Permet d'indiquer si le contenu du script doit être interprété comme un template.

(string) `packName`
:   Les scripts peuvent être concaténés afin de réduire le nombre de fichiers
    chargés par le client. Dans ce cas, tous les scripts ajoutés avec un même
    identifiant `packName` seront concaténés pour ne donner qu'un seul fichier à
    charger. Des `packName` différents peuvent être utilisés pour faire des
    scripts qui seront servis chacun dans un fichier unique et distinct par
    `packName`.

## Valeur de Retour {#core-ref:5e557c4d-f9e7-496d-96ae-8ab287b070eb}

La méthode retourne une chaîne de caractère non vide avec l'emplacement du
script JavaScript ajouté, ou une chaîne de caractère vide si l'emplacement du
script n'est pas valide.

## Erreurs / Exceptions {#core-ref:5c1f9f56-615a-4e6e-a17c-ac43b40ce293}

N/A

## Historique {#core-ref:67bddef5-2b08-4c06-a75f-a4583bcb77e6}

N/A

## Exemples {#core-ref:c46f66ec-69d7-4f1e-9d2b-47782fb48a2e}

- Exemple #1

Script JavaScript `MY_APP/Layout/js_1.js` :


    [javascript]
    alert('This is JS#1');

Script JavaScript `MY_APP/Layout/js_2.js` :


    [javascript]
    alert('This is JS#2');

Contrôleur de l'action `MY_ACTION` (`my_action.php`) de l'application `MY_APP` :


    [php]
    function my_action(Action &$action) {
        /* Get the Application of the current Action */
        $application = $action->parent;
        
        $application->addJsRef('MY_APP/Layout/js_1.js', false, 'my_js');
        $application->addJsRef('MY_APP:js_2.js', false, 'my_js');
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
        [JS:REF]
        <h1>Have you seen any alert lately?</h1>
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
        <script type="text/javascript" language="JavaScript" src="pack.php?type=js&pack=my_js&wv=4253" /></script>
        <h1>Have you seen any alert lately?</h1>
      </body>
    </html>

## Notes {#core-ref:70208a8e-d4ee-42dc-8626-ca2cbda0933f}

- Les scripts ajoutées via cette méthode seront servis avec une directive 
indiquant au client de mettre en cache le résultat obtenu et minimiser ainsi les
requêtes au serveur.
- L'expiration de la mise en cache est gérée via un argument `wv=<version>` dont
le numéro de version est incrémenté à chaque mise à jour d'un module.
- Les `packName` sont composés par utilisateur et sont mis en cache sur le 
navigateur de l'utilisateur : ils ne sont donc pas partagés et/ou mis en cache 
sur le serveur.

## Voir aussi {#core-ref:b925e618-7dc9-4fa5-ab65-3e7b556befa0}

- [`Application::addJsCode`](#core-ref:49a8e28b-f286-45d7-b9e0-cc3591a8efde)
- [`Application::getJSLink`](#core-ref:95056b5d-5002-4a5d-bc26-31595fad9afd)
