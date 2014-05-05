# Importer des documents par ligne de commande {#core-ref:1c97f553-dcba-454e-96a0-8059230065b3}

La commande [_wsh_][wsh] [`importDocuments`][importdoc] permet d'importer des
documents en ligne de commande. Les types suivants sont supportés par le script
:

*   [CVS][import_csv]
    *   Encodage : UTF-8,
    *   Séparateur de colonnes : `;`(point-virgule par défaut - configurable en <span class="flag from release">3.2.12</span>).
    *   Délimiteur de texte : ` ` (vide par défaut - configurable en <span class="flag from release">3.2.12</span>),
    [ODS][import_csv],
*   [XML][import_xml],
*   [ZIP][import_archive],
*   [TGZ][import_archive]

    ./wsh.php --api=importDocuments --help
    Import documents from description file
    Usage :
        --file=<the description file path>
    Options:
    --userid=<user system id or login name to execute function - default is (admin)>, default is '1'
    --analyze=<analyze only> [yes|no], default is 'no'
    --archive=<description file is an standard archive (not xml)> [yes|no], default is 'no'
    --log=<log file output>
    --policy=<policy import - 
        [update] to auto update same document (the default), 
        [add] to always create new document, 
        [keep] to do nothing if same document already present> [update|add|keep]
    --htmlmode=<analyze report mode in html> [yes|no], default is 'yes'
    --reinitattr=<reset attribute before import family update (deprecated)> [yes|no]
    --reset=<reset options> [default|parameters|attributes|structure|properties|enums] 
    --to=<email address to send report>
    --dir=<folder where imported documents are put>
    --strict=<don't import if one error detected> [yes|no], default is 'yes'
    --csv-separator=<character to delimiter fields - generaly a comma> use single character or "auto", default is ';'
    --csv-enclosure=<character to enclose fields - generaly double-quote> use single character or "auto", default is ''
    --csv-linebreak=<character sequence to be import like CRLF>, default is '\n'
    --help (Show usage) 

L'option `--analyze=yes` permet de réaliser une importation *à blanc*. Dans ce
cas, l'importation est réalisée mais aucune modification n'est enregistrée en
base de données. Cela permet de récupérer le rapport d'importation :

     ./wsh.php --api=importDocuments --analyze=yes --htmlmode=yes --file=animal.xml > report.html

Pour envoyer le rapport par courriel, l'option `--to` permet de désigner
l'adresse du destinataire :

     ./wsh.php --api=importDocuments --to=someone@somewhere.org --file=animal.xml 

Pour avoir un fichier de log, l'option `--log` permet de spécifier l'emplacement
auquel enregistrer le fichier de log :

    ./wsh.php --api=importDocuments --file=animaux.xml --htmlmode=yes --analyze=yes --log=/var/tmp/log.txt
    $ cat /var/tmp/log.txt
    IMPORT BEGIN OK : 02/07/2010 17:10:29
    IMPORT DOC OK : [title:Isabelle] [id:0] [action:added] [changes:{nom:Isabelle}{espèce:1132}] [message:Isabelle à ajouter] 
    IMPORT DOC OK : [title:Alli2 Agouti] [id:3296] [action:updated] [changes:] [message:]
    IMPORT DOC KO : [title:] [id:0] [action:ignored] [changes:] [message:] [error:DOMDocument::load(): Opening and ending tag mismatch: r line 5 and zoo_animal in /var/tmp/xmlsplit4c2e10009374f/3299.xml, line: 10DOMDocument::load(): Premature end of data in tag zoo_animal line 2 in /var/tmp/xmlsplit4c2e10009374f/3299.xml, line: 11]
    IMPORT COUNT OK : 2
    IMPORT COUNT KO : 1
    IMPORT END OK : 02/07/2010 17:10:31

## Options pour les importations de familles {#core-ref:330f2421-4cc5-458a-8502-c0d715e612a6}

L'option `reset` permet d'influer sur l'interprétation de l'importation en
rajoutant des [instructions de `RESET`][resetfam] au(x) définition(s) de famille(s).

L'option `reset` rajoute l'instruction `RESET` correspondant après chaque
`BEGIN` du fichier. Toutes les familles définies ont cette instruction
ajoutée.

    ./wsh.php --api=importDocuments --file=my_family.csv --reset=attributes

Cette option peut être multivaluée pour indiquer plusieurs instructions de
`RESET`.

    ./wsh.php --api=importDocuments --file=my_family.csv --reset[]=enums --reset[]=default

L'option `reinitattr` est dépréciée. Cette option est équivalente à
`reset=attributes`.

## Gestion des erreurs {#core-ref:1ab32c44-3233-4de6-bede-97f0aa58e617}

Si une seule erreur est détectée sur le fichier d'importation, aucun document du
fichier d'importation ne sera ajouté ou modifié.

<!-- links -->
[wsh]:          #core-ref:bab8c1c9-fe71-4629-9773-5cd67a8693bf
[import_csv]:   #core-ref:2fb3284a-2424-44b2-93ae-41dc3969e093
[import_xml]:   #core-ref:81ad5a48-4c0f-468b-90ed-fe462fba7b96
[import_archive]: #core-ref:021b7db1-7baf-48c4-8eb9-4a388355dd86
[resetfam]:     #core-ref:5c661733-772d-42b8-8b3e-b70453ddfd33
[importdoc]:    #core-ref:a14d9475-0431-4aa3-853d-810b61e355a7