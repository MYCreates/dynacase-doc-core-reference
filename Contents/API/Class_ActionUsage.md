# Classe ActionUsage {#core-ref:7a8932eb-a59f-482a-9991-4ee1c634eae4}

La classe `ActionUsage` gère la définition et la validation des paramètres d'une
action.

Elle permet de spécifier et contrôler la présence de paramètres requis ou
optionnels, et de retourner un texte d'aide précisant les paramètres requis
lorsque la validation échoue.

La classe `ActionUsage` hérite de la classe [`ApiUsage`][ApiUsage].

## Constructeur {#core-ref:a5a32b02-97ef-4e48-a4b6-cabb8b77c563}

    [php]
    new ActionUsage ( Action & $action )

### Liste des paramètres {#core-ref:9a2dd12c-9dcc-4062-8a49-b6051cd9345c}

(Action) `action`
:   L'objet `$action` courant.

### Exemples {#core-ref:39185bc0-477f-4ab5-92d4-199212e582a7}

    [php]
    function mon_action(Action & $action) {
        $usage = new ActionUsage($action);
        $docId = $usage->addRequiredParameter("id", "document identifier");
        $usage->verify();
        
        doSomethingWith($docId);
    }

### Notes {#core-ref:9429d447-08ae-40db-a4cd-e3f24eb41b21}

Cette classe est utilisée pour les actions déclenchés par le serveur web ou via
le script [wsh][wsh].

Lorqu'il est déclenché par le serveur web, les paramètres sont récupérées par
les variables suivantes dans l'ordre indiqué :

1.   [`$_GET`][GET] : Variable HTTP GET
2.   [`$_POST`][POST] : Variable HTTP POST (formulaire)
3.   [`$_FILES`][FILES] : <span class="flag from release inline">3.2.21</span>  Formulaire avec fichier

Les variables GET sont prioritaires aux variables POST qui sont prioritaires aux
variables FILES.

### Voir aussi {#core-ref:2895e6de-d570-491e-8a13-d0840b511e57}

- [`ApiUsage`][ApiUsage]

<!-- links -->
[ApiUsage]: #core-ref:dac6d107-3e77-48ba-8912-ffccd0061cbf
[wsh]:      #core-ref:bab8c1c9-fe71-4629-9773-5cd67a8693bf
[GET]:      http://php.net/manual/fr/reserved.variables.get.php "php.net : super globale $_GET"
[POST]:     http://php.net/manual/fr/reserved.variables.post.php "php.net : super globale $_POST"
[FILES]:     http://php.net/manual/fr/reserved.variables.files.php "php.net : super globale $_FILES"