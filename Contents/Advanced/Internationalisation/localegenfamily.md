# Extraction des clés pour les familles de document {#core-ref:f5872ef4-4170-11e3-ba58-48f953959281}

La description de la [structure de famille][famdecl] est définie dans une
fichier CSV ou ODS. Ce fichier est utilisable pour produire un catalogue de traduction.

Exemple : TestSimple.ods

| BEGIN |             | Test simple |                |     | TST_SIMPLE |       |     |     |      |      |         |                                                     |
| ----- | ----------- | ----------- | -------------- | --- | ---------- | ----- | --- | --- | ---- | ---- | ------- | --------------------------------------------------- |
|       | idattr      | idframe     | label          | T   | A          | type  | ord | vis | need | link | phpfile | phpfunc                                             |
| ATTR  | TST_FR_HTML |             | Identification | N   | N          | frame | 100 | W   |      |      |         |                                                     |
| ATTR  | TST_TITLE   | TST_FR_HTML | Titre          | Y   | N          | text  | 110 | W   | Y    |      |         |                                                     |
| ATTR  | TST_SIZE    | TST_FR_HTML | Taille         | N   | N          | enum  | 120 | W   |      |      |         | XL&#124; Very large,L &#124; Large,M &#124;Medium,S |

Le programme [`xgettextFamily`][buildtools] permet d'extraire les clefs de traduction depuis un
ou plusieurs fichiers de description de famille (CSV ou ODS).
<span class="flag from release">3.2.12</span>

    ./buildTools/xgettextFamily -o / TestSimple.ods
    
    myLocaleDirectory/TST_SIMPLE.pot wrote.

produit le fichier `locale/TST_SIMPLE.pot`. L'identifiant de la famille sert
pour le nom du fichier et pour les clefs de traduction. 

L'option de sortie `-o` doit renseigner un répertoire accessible en écriture.

La production du fichier
temporaire sert pour construire les catalogues dans les langues voulues.

Il est possible de déclarer les fichiers d'entrées via le pipe.

    find . -name "family*.csv" | ../buildtools/xgettextFamily -o myLocaleDirectory -f-

Cela produit un fichier `.pot` par famille.

Les options de `xgettext` **ne sont pas utilisables** pour modifier le fichier produit.

Extrait du fichier généré "TST_SIMPLE.pot" avec la famille décrite ci-dessus :

    msgid ""
    msgstr ""
    "Project-Id-Version: TST_SIMPLE \n"
    "Report-Msgid-Bugs-To: \n"
    "PO-Revision-Date: 2013-10-25T14:59:04+02:00\n"
    "Last-Translator: Automatically generated\n"
    "Language-Team: none\n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"
    "Language: \n"
    
    #, fuzzy, (TestSimple.ods.csv)
    msgid "TST_SIMPLE#title"
    msgstr "Test simple"
    
    #: TestSimple.ods.csv
    #, fuzzy, (TestSimple.ods.csv)
    msgid "TST_SIMPLE#tst_fr_html"
    msgstr "Cadre 1"
    
    #: TestSimple.ods.csv
    #, fuzzy, (TestSimple.ods.csv)
    msgid "TST_SIMPLE#tst_title"
    msgstr "Titre"
    
    #: TestSimple.ods.csv
    #, fuzzy, (TestSimple.ods.csv)
    msgid "TST_SIMPLE#tst_size"
    msgstr "Taille"
    
    #, fuzzy, (TestSimple.ods.csv)
    msgid "TST_SIMPLE#tst_size#XL"
    msgstr "Very large"
    ...

Les clefs suivantes sont générée :

`FAMNAME#title`
:   Titre de la famille FAMNAME

`FAMNAME#<attrid>`
:   Libellé de l'attribut `<attrid>` de la famille FAMNAME

`FAMNAME#<attrid>#ENUMVALUE`
:   Libellé de la valeur ENUMVALUE de l'énuméré `<attrid>` de la famille FAMNAME.
    **Note** : Les traductions des énumérés peuvent être modifiées par
    l'administrateur depuis l'interface du centre d'administration. Les traductions
    des énumérés faite par l'interface sont toujours prioritaires aux traductions
    faites dans les catalogues livrés par le module.

`FAMNAME#<attrid>#OPTIONKEY`
:   Traduction des valeurs de certaines [options d'attributs][optionattr] 
    sensibles à la localisation.
    Les options prises en compte sont :  "elabel", "ititle", "submenu", 
    "ltitle", "eltitle", "elsymbol", "lsymbol", "showempty".

<!-- link -->
[wikiGettext]:       http://fr.wikipedia.org/wiki/GNU_gettext "Gettext sur Wikipédia"
[phpGettext]:        http://www.php.net/manual/fr/function.gettext.php "gettext sur php.net"
[actions]:           #core-ref:e67d8aeb-939c-46e3-9be8-6fc3ba75ebc2 "Action Dynacase"
[wsh]:               #core-ref:4df1314f-9fdd-4a7f-af37-a18cc39f3505 "Script Dynacase"
[gencatalog]:        #core-ref:2c163f00-8e94-4736-86f2-bb51352c52aa
[pgettext]:          http://www.gnu.org/software/gettext/manual/html_node/Contexts.html "Contexte dans gettext"
[ngettext]:          http://www.php.net/manual/fr/function.ngettext.php "ngettext sur php.net"
[layout]:           #core-ref:5f4a2f4b-9ceb-42db-8ac1-2a7baa621ce2
[xgettext]:         http://www.gnu.org/software/gettext/manual/html_node/xgettext-Invocation.htm "xgettext reference"
[famdecl]:          #core-ref:cfc7f53b-7982-431e-a04b-7b54eddf4a75
[gettextutil]:      http://www.gnu.org/software/gettext/manual/html_node/index.html#Top
[optionattr]:       #core-ref:16e19c90-3233-11e2-a58f-6b135c3a2496 "Options d'attribut"
[buildtools]:       https://github.com/Anakeen/dynacase-buildtools   "Source BuildTools"