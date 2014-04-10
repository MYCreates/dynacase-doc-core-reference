# importDocuments {#core-ref:a14d9475-0431-4aa3-853d-810b61e355a7}

## Description {#core-ref:64029eb5-c117-455e-a3fb-0a65e5f08b69}

Le script `importDocuments` permet d'importer des définitions de familles ou
d'importer des documents.

Voir aussi :

* [Importer des documents par ligne de commande][wshImportDocuments]

## Usage {#core-ref:22deded0-83e1-4fd5-89a6-91b3820b8649}

**`file` (obligatoire)**
:   Le fichier à importer.
    
    Le fichier peut être au format CSV, XML ou bien une archive (si l'option
    `--archive=yes` est utilisée).

`analyze` (default `no`)
:   Permet d'effectuer (`yes`) seulement une validation du format et de la
    structure du fichier importé.

`archive` (default `no`)
:   Permet d'indiquer si le fichier est une archive (`yes`) ou bien un fichier
    d'import texte (`no`).
    
    Voir [Importation d'archive][importation_archive] pour le format de ces
    archives.

`log`
:   Permet de spécifier un fichier de log dans lequel seront inscrits les
    résultats de l'import.

`policy` (default `update`)
:   Permet de spécifier le comportement lorsqu'on importe un document qui existe
    déjà.
    
    La détection des documents existants est spécifiée par la directive d'import
    [`KEYS`][KEYS].
    
    Les valeurs possibles pour l'option `policy` sont :
    
    `add`
    :   Un nouveau document est créé systématiquement, même si un document
        existant est détecté.
    
    `keep`
    :   Si un document existant est détecté, alors le document existant est
        conservé en l'état, et l'import ne crée pas un nouveau document.
    
    `update`
    :   Si un document existant est détecté, alors le document existant est
        mis à jour avec les données du document importé.

`htmlmode` (default `yes`)
:   Permet de spécifier si les messages du fichier de log doivent être inscrits
    au format HTML (`yes`) ou au format texte (`no`).

`reset[]`
:   Permet de spécifier les éléments qui doivent être supprimés, ou redéfinis,
    lors de l'import d'une définition de famille.
    
    Les valeurs possibles sont celles de l'
    [instruction de `RESET`][RESET].
    
    Cette option peut être utilisée plusieurs fois afin de spécifier plusieurs
    valeurs.

`to`
:   Permet d'envoyer le log au format HTML à l'adresse mail indiquée par cette
    option.

`dir`
:   Identifiant, ou nom logique, d'un répertoire Dynacase dans lequel seront
    insérés les documents importés.

`strict` (default `yes`)
:   Permet de spécifier (`yes`) si une erreur d'import d'un document invalide
    l'import de tous les documents. Si le mode strict est désactivé (`no`),
    alors une erreur sur un document n'empêchera pas les autres documents
    d'êtres importés.

`csv-separator` (default `;`) <span class="flag from release">3.2.12</span>
:   Dans le cas d'importation en fichier [csv][CSV] permet d'indiquer le caractère de
    séparation des champs. Généralement le caractère est `,` (virgule) ou 
    `;` (point-virgule).  
    Si la valeur est `auto`, le caractère de séparation parmi `,` et `;` est 
    trouvé automatiquement.

`csv-enclosure` (default `vide`) <span class="flag from release">3.2.12</span>
:   Dans le cas d'importation en fichier [csv][CSV] permet d'indiquer le caractère de
    de délimiteur de texte. Généralement le caractère est `"` (double quote) ou 
    `'` (simple quote)  
    Si la valeur est `auto`, le caractère de séparation parmi `"` et `'` est 
    trouvé automatiquement.


`csv-linebreak` (default "`\n` - 2 caractères `\` et `n`") <span class="flag from release">3.2.12</span>
:   Dans le cas d'importation en fichier [csv][CSV] permet d'indiquer une séquence de 
    caractère indiquant un retour chariot (CRLF). Si `csv-enclosure` n'est pas 
    vide, le retour chariot (caractère `"\n"`) est pris en compte quelque soit 
    la valeur de cet argument. Cet argument permet en plus d'indiquer une 
    séquence qui sera interprétée comme un retour à la ligne.


## Limite d'usage {#core-ref:ab8856e9-1850-46d9-ae22-20fb54f9c078}

Aucunes.

<!-- links -->
[wshImportDocuments]:   #core-ref:1c97f553-dcba-454e-96a0-8059230065b3
[importation_archive]:  #core-ref:021b7db1-7baf-48c4-8eb9-4a388355dd86
[KEYS]:                 #core-ref:7eefc8e7-16a6-4188-99d5-c2c9d817a1fe
[RESET]:                #core-ref:5c661733-772d-42b8-8b3e-b70453ddfd33
[CSV]: http://fr.wikipedia.org/wiki/Comma-separated_values "Comma-separated values sur wikipedia"