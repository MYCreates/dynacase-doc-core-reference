# Extraction des clés pour les templates {#core-ref:0294a176-4171-11e3-b364-cffb8e583c34}


Le programme [`xgettextLayout`][buildtools] permet d'extraire les clefs de
traduction depuis des fichier de template texte et openDocument Text.<span
class="flag from release">3.2.12</span>

    ./buildTools/xgettextLayout -o myLayouts.pot file1.html file2.xml file3.js file4.odt ...

Les clefs '[TEXT:...]' sont extraites et indiquées dans le fichier `.pot` produit.
De même les clefs avec contexte `|TEXT(ctx):...  ]` sont aussi extraites avec leur 
contexte.

Les options de `xgettext` sont utilisables pour modifier le fichier produit.

    ./buildtools/xgettextLayout --omit-header -o myLayouts.pot test.html

Il est possible de déclarer les fichiers d'entrées via le pipe.

    find . -name "*xml" | ../buildtools/xgettextLayout -o myLayouts.pot -f-


<!-- link -->
[wikiGettext]:       http://fr.wikipedia.org/wiki/GNU_gettext "Gettext sur Wikipédia"
[phpGettext]:        http://www.php.net/manual/fr/function.gettext.php "gettext sur php.net"
[actions]:           #core-ref:e67d8aeb-939c-46e3-9be8-6fc3ba75ebc2 "Action Dynacase"
[wsh]:               #core-ref:4df1314f-9fdd-4a7f-af37-a18cc39f3505 "Script Dynacase"
[gencatalog]:        #core-ref:2c163f00-8e94-4736-86f2-bb51352c52aa
[pgettext]:          http://www.gnu.org/software/gettext/manual/html_node/Contexts.html "Contexte dans gettext"
[ngettext]:          http://www.php.net/manual/fr/function.ngettext.php "ngettext sur php.net"
[layout]:            #core-ref:5f4a2f4b-9ceb-42db-8ac1-2a7baa621ce2
[xgettext]:          http://www.gnu.org/software/gettext/manual/html_node/xgettext-Invocation.htm "xgettext reference"
[famdecl]:           #core-ref:cfc7f53b-7982-431e-a04b-7b54eddf4a75
[gettextutil]:       http://www.gnu.org/software/gettext/manual/html_node/index.html#Top
[buildtools]:       https://github.com/Anakeen/dynacase-buildtools   "Source BuildTools"