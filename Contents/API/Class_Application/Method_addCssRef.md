# Application::addCssRef {#core-ref:4bba8a6b-8002-4c0a-8ac7-70d75b31b02b}

<div class="short-description">
Ajouter un lien vers une feuille de style CSS à un template.
</div>

<!-- <div class="applicability"></div> -->

## Description {#core-ref:03c18ede-a3be-4518-adbd-265b2ea74da7}


    [php]
    string addCssRef ( string $ref [, bool $needparse = false [, string $packName = '']] )

Permet d'ajouter un lien vers une feuille de style CSS qui sera insérée dans les
éléments `[CSS:REF]` des templates utilisées lors du rendu d'une vue.

## Avertissements {#core-ref:22003acd-a383-4c2c-928a-3ba9f8e53b88}

N/A

## Liste des paramètres {#core-ref:b6d1170a-db84-4ab1-8b18-504bd4b445b0}

(string) `ref`
:   La référence à la feuille de style CSS.
    La référence peut être : un chemin d'accès relatif au contexte Dynacase (ex.
    `MY_APP/Layout/my_css.css`, `my_css.css`), un chemin d'accès relatif au
    répertoire `Layout` du style actuellement appliqué, un chemin d'accès
    relatif au répertoire `Layout` de l'application courante ou d'une
    application particulière (ex. `MY_APP:my_css.css`), une URL (ex.
    `http://www.example.net/my_css.css`).

(bool) `needparse`
:   Permet d'indiquer si le contenu de la feuille de style doit être interprété 
    comme un template.

(string) `packName`
:   Les feuilles de style peuvent être concaténées afin de réduire le nombre de 
    fichiers chargés par le client. Dans ce cas, toutes les feuilles de style
    ajoutées avec un même identifiant `packName` seront concaténées pour ne
    donner qu'un seul fichier à charger. Des `packName` différents peuvent être
    utilisés pour faire des groupes de feuilles de style qui seront servis
    chacun dans un fichier unique et distinct par `packName`.

## Valeur de Retour {#core-ref:27dbd4dd-485d-4f2e-abcd-ab0ac66d73c6}

La méthode retourne une chaîne de caractères non vide avec l'emplacement de la
feuille de style CSS ajoutée, ou une chaîne de caractères vide si l'emplacement
de la feuille de style n'est pas valide.

## Erreurs / Exceptions {#core-ref:41788c64-aa2b-4f2f-9470-46c6cd24fe04}

N/A

## Historique {#core-ref:ac4c2011-7104-4bff-9938-571a44bbeff1}

N/A

## Exemples {#core-ref:70b7105c-4a8d-4c6a-a23e-ecf3b44d285c}

- Exemple #1

Feuille de style CSS `MY_APP/Layout/css_1.css` :


    [css]
    .question {
        color: blue;
    }

Feuille de style CSS `MY_APP/Layout/css_2.css` :


    [css]
    .question {
        voice-family: Bridgekeeper, male;
    }

Contrôleur de l'action `MY_ACTION` (`my_action.php`) de l'application `MY_APP` :


    [php]
    function my_action(Action &$action) {
        /* Get the Application of the current Action */
        $application = $action->parent;
        
        $application->addCssRef('MY_APP/Layout/css_1.css', false, 'my_css');
        $application->addCssRef('MY_APP:css_2.css', false, 'my_css');
    }

Vue de l'action `MY_ACTION` (`Layout/my_action.xml`) :


    [html]
    <!DOCTYPE html>
    <html>
      <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <title>My Action</title>
        [CSS:REF]
      </head>
      <body>
        <h1 class="question">What is your favourite colour?</h1>
      </body>
    </html>


Résultat du rendu de la vue de l'action :


    [html]
    <!DOCTYPE html>
    <html>
      <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <title>My Action</title>
        <link rel="stylesheet" type="text/css" href="pack.php?type=css&pack=my_css&wv=4253" />
      </head>
      <body>
        <h1 class="question">What is your favourite colour?</h1>
      </body>
    </html>

## Notes {#core-ref:72360e3f-5350-4a2f-a6a7-6ebbd2e80125}

- Les feuilles de style ajoutées via cette méthode seront servies avec une 
directive indiquant au client de mettre en cache le résultat obtenu et minimiser
ainsi les requêtes au serveur.
- L'expiration de la mise en cache est gérée via un argument `wv=<version>` dont
le numéro de version est incrémenté à chaque mise à jour d'un module.
- Les `packName` sont composés par utilisateur et sont mis en cache sur le 
navigateur de l'utilisateur : ils ne sont donc pas partagés et/ou mis en cache 
sur le serveur.

## Voir aussi {#core-ref:0f8f1e52-37fb-4c9e-9c71-051b98ffeac2}

- [`Application::addCssCode`](#core-ref:5480ed21-5027-45c3-9efe-3897784b8865)
- [`Application::getCSSLink`](#core-ref:7a7be83f-fbc2-4f45-8115-bc295321bcf4)
