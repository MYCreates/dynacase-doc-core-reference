# Application::addJsCode {#core-ref:49a8e28b-f286-45d7-b9e0-cc3591a8efde}

<div class="short-description">
Ajouter du code JavaScript à un template.
</div>

<!-- <div class="applicability"></div> -->

## Description {#core-ref:f066ff5a-f356-4b92-af63-b9f98565d924}


    [php]
    void addJsCode ( string $code )

Permet d'ajouter des instructions JavaScript (`code`) qui seront insérées dans
les éléments `[JS:CODE]` des templates utilisées lors du rendu d'une vue.

## Avertissements {#core-ref:307520c6-c9b7-4089-9716-da6fabed51b3}

N/A

## Liste des paramètres {#core-ref:47953186-b7e2-4921-802a-cf62848b5559}

(string) `code`
:   Bloc de code d'instructions JavaScript.

## Valeur de Retour {#core-ref:91be3731-622f-43c3-957b-782353211240}

La méthode ne retourne pas de valeur.

## Erreurs / Exceptions {#core-ref:e035dd1d-79a7-4e1b-aa36-00e54a726443}

N/A

## Historique {#core-ref:af32718c-8fc7-43db-ae5b-a73c158447fc}

N/A

## Exemples {#core-ref:2d4be354-a439-428e-ac7e-6a4b6d5d6e1a}

- Exemple #1

Contrôleur de l'action `MY_ACTION` (`my_action.php`) :


    [php]
    function my_action(Action &$action) {
        /* Get the Application of the current Action */
        $application = $action->parent;
        $jsCode = <<<'JS'
    alert('Hello world');
    JS;
        $application->addJsCode($jsCode);
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
        <script type="text/javascript">
    [JS:CODE]
        </script>
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
        <script type="text/javascript" language="JavaScript">
    alert('Hello world');
        </script>
        <h1>Have you seen any alert lately?</h1>
      </body>
    </html>

## Notes {#core-ref:548bfb05-6bdd-49d2-96af-5729cefbd06e}

N/A

## Voir aussi {#core-ref:ca7ba655-2ca2-4aec-adb5-6916c7382f32}

- [`Application::addJsRef`](#core-ref:b4b041aa-2649-498d-ace7-52131053c7db)
- [`Application::getJSLink`](#core-ref:95056b5d-5002-4a5d-bc26-31595fad9afd)
