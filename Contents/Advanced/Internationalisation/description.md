# Description du principe de traduction {#core-ref:0a03a448-736d-4caa-ac95-72e735aeea6f}

Les traductions utilisent la bibliothèque [getText][wikiGettext] qui est un
standard pour la gestion des programmes multilingues.

Lors de l'initialisation d'une [action][actions] ou d'un [script][wsh], le
catalogue correspondant à la langue de l'utilisateur est chargé. La langue de
l'utilisateur est choisie lors de sa connexion sur l'écran d'identification. Ce
choix de langue est stockée dans le paramètre applicatif `CORE_LANG`. Ce
paramètre est modifié à chaque nouvelle connexion de l'utilisateur.

Le catalogue qui est utilisé contient tous les textes traduits de tous les
modules installés. Il est dont important d'avoir des textes qui ne sont pas déjà
utilisés par un autre module pour éviter d'écraser les traductions d'un autre
module, ce qui pourrait dégrader la traduction de ce module.

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