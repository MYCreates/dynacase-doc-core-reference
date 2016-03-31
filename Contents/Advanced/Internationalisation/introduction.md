# Internationalisation {#core-ref:8f3ad20a-4630-4e86-937b-da3fa26ba423}

Ce chapitre indique comment utiliser des textes localisés dans vos applications
et dans toutes les interfaces homme-machine.

La processus d'internationalisation se déroule en plusieurs étapes :

1. extraction des éléments à traduire et production d'un catalogue par langue cible,
1. traduction des éléments,
1. publication des catalogues dans le contexte.

Une fois ces étapes suivies, les traductions se font dans vos programmes.

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