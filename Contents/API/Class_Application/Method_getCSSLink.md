# Application::getCSSLink {#core-ref:7a7be83f-fbc2-4f45-8115-bc295321bcf4}

<div class="short-description">
Récupérer le lien de téléchargement (URL) d'une feuille de style CSS.
</div>

<!-- <div class="applicability"></div> -->

## Description {#core-ref:e7994e66-c164-414f-8d03-6680f4c16ad3}


    [php]
    string getCSSLink ( string $ref [, bool $needparse = false [, string $packName = '']] )

Permet de retourner le lien de téléchargement (URL) d'une feuille de style CSS
servie par Dynacase avec gestion de la version du cache (Voir [Notes](#core-ref
:9C309FEA-C54A-48C3-AD9C-03F20D532D1D)).

## Avertissements {#core-ref:06fd2b30-72c3-4e44-8656-588edfcae2ca}

N/A

## Liste des paramètres {#core-ref:6469fdad-a30e-4e74-a191-9c7392b3f56c}

(string) `ref`
:   La référence à la feuille de style CSS.
    La référence peut être : un chemin d'accès relatif au contexte Dynacase (ex.
    `MY_APP/Layout/my_css.css`, `my_css.css`), un chemin d'accès relatif au
    répertoire `Layout` du style   actuellement appliqué, un chemin d'accès
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

## Valeur de Retour {#core-ref:8b51af9c-cc86-4a76-a312-38bb27b1d737}

La méthode retourne une chaîne de caractère non vide avec le lien de
téléchargement de la feuille de style CSS ajoutée, ou une chaîne de caractère
vide si l'emplacement de la feuille de style n'est pas valide.

## Erreurs / Exceptions {#core-ref:f8fbeec1-db4a-4612-90e3-01fb3fedc38d}

N/A

## Historique {#core-ref:dacabe14-a2fc-4641-a06a-5e356771db03}

N/A

## Exemples {#core-ref:56cd86da-97fa-4d24-8bc6-e1491883d5f5}

- Exemple #1

Feuille de style CSS `MY_APP/Layout/css_1.css` :


    [css]
    .question {
        color: blue;
        voice-family: Bridgekeeper, male;
    }

Contrôleur de l'action `MY_ACTION` (`my_action.php`) :


    [php]
    function my_action(Action &$action) {
        /* Get the Application of the current Action */
        $application = $action->parent;
        
        $cssLink = $application->getCSSLink('MY_APP/Layout/css_1.css');
        
        $action->lay->set('CSS_LINK', htmlspecialchars($cssLink));
    }

Vue de l'action `MY_ACTION` (`Layout/my_action.xml`) :


    [html]
    <!DOCTYPE html>
    <html>
      <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <title>My Action</title>
        <link rel="stylesheet" type="text/css" href="[CSS_LINK]" />
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
        <link rel="stylesheet" type="text/css" href="MY_APP/Layout/css_1.css?wv=4253" />
      </head>
      <body>
        <h1 class="question">What is your favourite colour?</h1>
      </body>
    </html>

## Notes {#core-ref:9c309fea-c54a-48c3-ad9c-03f20d532d1d}

- Les feuilles de style ajoutées via cette méthode seront servies avec une 
directive indiquant au client de mettre en cache le résultat obtenu et minimiser
 ainsi les requêtes au serveur.
- L'expiration de la mise en cache est gérée via un argument `wv=<version>` dont
 le numéro de version est incrémenté à chaque mise à jour d'un module.
- Les `packName` sont composés par utilisateur et sont mis en cache sur le 
navigateur de l'utilisateur : ils ne sont donc pas partagés et/ou mis en cache 
sur le serveur.

## Voir aussi {#core-ref:c9bc22f8-4d83-4d16-aefb-684b25f7cbc4}

- [`Application::addCssCode`](#core-ref:5480ed21-5027-45c3-9efe-3897784b8865)
- [`Application::addCssRef`](#core-ref:4bba8a6b-8002-4c0a-8ac7-70d75b31b02b)
