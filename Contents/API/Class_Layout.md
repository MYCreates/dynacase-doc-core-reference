# Classe Layout {#core-ref:9f9edc1b-17a5-4f54-86ee-69e33016fe18}

La classe `Layout` permet d'évaluer un [fichier de
template][UtilisationDesTemplates], ou une chaîne de caractère respectant la
syntaxe des templates, et de produire le résultat.

## Constructeur {#core-ref:d0038e8d-b915-4d84-ac28-4f6c0aeb20a0}

    [php]
    new Layout ( string $caneva = "", (Action) & $action = null, string $template = "[OUT]" )

### Liste des paramètres {#core-ref:7e440aab-5593-4977-b878-72a8edfdc7a9}

(string) `caneva`:
:   Chemin d'accès à un fichier de template textuel, c'est-à-dire non binaire
    (e.g. `FOO/Layout/bar.html`). Le contenu du fichier spécifié sert alors de
    template.

(Action) `action`:
:   L'objet `Action` associée à l'évaluation du template. 
    L'action rend disponible les clefs des paramètres applicatifs.

(string) `template`:
:   Contenu du template. Si `caneva` n'est pas spécifié, ou n'est pas
    accessible, alors le contenu du template sera affecté avec cette valeur.
    Dans le cas contraire, cette valeur est ignorée.

### Exemples {#core-ref:eeb538d8-f35c-4e8b-9f36-8b7cbe350f85}

- Exemple #1

Instanciation à partir d'un fichier de template `FOO/Layout/bar.xml` :

    [php]
    $layout = new Layout("FOO/Layout/bar.xml");

- Exemple #2

Instanciation à partir d'un template contenu dans une chaîne de caractères :

    [php]
    $layout = new Layout(
        "",
        null,
        "<p>Bonjour [NOM]&nbsp;[PRENOM].</p><p>La température est actuellement de [TEMPC]°C.</p>"
    );
    $layout->eSet("NOM","Watson");
    $layout->eSet("PRENOM","Robert");
    $layout->eSet("TEMPC","23");
    print $layout->gen();

### Voir aussi {#core-ref:bebe6da9-59d8-459e-9e60-6798800936e4}

- [Utilisation des templates][UtilisationDesTemplates]

<!-- links -->
[UtilisationDesTemplates]: #core-ref:5f4a2f4b-9ceb-42db-8ac1-2a7baa621ce2