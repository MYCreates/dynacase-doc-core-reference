# Application::addCssCode {#core-ref:5480ed21-5027-45c3-9efe-3897784b8865}

<div class="short-description">
Ajouter du code CSS à un template.
</div>

<!-- <div class="applicability"></div> -->

## Description {#core-ref:08686c08-8a9e-4f28-a7bb-b4eb5dc39850}


    [php]
    void addCssCode ( string $code )

Permet d'ajouter des instructions CSS (`code`) qui seront insérées dans les
éléments `[CSS:CODE]` des templates utilisées lors du rendu d'une vue.

## Avertissements {#core-ref:2bed3980-13be-42b5-9380-a7c6896ef89c}

N/A

## Liste des paramètres {#core-ref:0253405e-3d82-4724-ad06-eb0d787dc51d}

(string) `code`
:   Bloc d'instructions CSS.

## Valeur de Retour {#core-ref:0b1eb3a8-638d-44e3-b0d3-60724f862145}

La méthode ne retourne pas de valeur.

## Erreurs / Exceptions {#core-ref:ccc19b74-123a-4fa4-9e5a-72b136bdda30}

N/A

## Historique {#core-ref:5d450b47-140b-4e43-9e73-eb1d4f5b5eca}

N/A

## Exemples {#core-ref:d3c6eb9c-be92-43c8-a144-c360d81faa1b}

- Exemple #1

Contrôleur de l'action `MY_ACTION` (`my_action.php`) :


    [php]
    function my_action(Action &$action) {
        /* Get the Application of the current Action */
        $application = $action->parent;
    
        $cssCode = <<<'CSS'
    .question {
        color: blue;
        voice-family: Bridgekeeper, male;
    }
    CSS;
    
        $application->addCssCode($cssCode);
    }

Vue de l'action `MY_ACTION` (`Layout/my_action.xml`) :


    [html]
    <!DOCTYPE html>
    <html>
      <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <title>My Action</title>
        <style type="text/css">
    [CSS:CODE]
        </style>
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
        <style type="text/css">
    .question {
        color: blue;
        voice-family: Bridgekeeper, male;
    }
        </style>
      </head>
      <body>
        <h1 class="question">What is your favourite colour?</h1>
      </body>
    </html>

## Notes {#core-ref:9f333924-790c-4332-88da-cd2e7539fc90}

N/A

## Voir aussi {#core-ref:46f3a61e-2ff9-490a-8c9e-69f01df5e823}

- [`Application::addCssRef`][addCSSRef]
- [`Application::getCSSLink`][getCSSLink]

<!-- links -->

[addCSSRef]: #core-ref:4bba8a6b-8002-4c0a-8ac7-70d75b31b02b
[getCSSLink]: #core-ref:7a7be83f-fbc2-4f45-8115-bc295321bcf4
