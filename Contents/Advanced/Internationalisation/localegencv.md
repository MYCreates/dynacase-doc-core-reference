# Extraction des clés pour les contrôles de vues {#core-ref:e01fa01e-4170-11e3-816c-60d7dc830245}

Les libellés des vues des [contrôles de vues][cvdoc] peuvent être traduits.

Le programme [`xgettextFamily`][buildtools] permet d'extraire les clefs de traduction depuis un
fichier de description de contrôle de vue.<span class="flag from release">3.2.12</span>

    ./buildTools/xgettextFamily -o myLocaleDirectory someCVDoc.csv
    
    myLocaleDirectory/MYCV.pot wrote

Les options de `xgettext` **ne sont pas utilisables** pour modifier le fichier
produit.

L'option de sortie `-o` doit renseigner un répertoire accessible en écriture.

Le fichier de sortie produit est le nom logique du document de contrôle de vue.

Exemple de fichier de traduction produits :

    msgid ""
    msgstr ""
    "Project-Id-Version: CV_IUSER_ACCOUNT \n"
    "Report-Msgid-Bugs-To: \n"
    "PO-Revision-Date: 2013-10-25T16:08:15+02:00\n"
    "Last-Translator: Automatically generated\n"
    "Language-Team: none\n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"
    "Language: \n"
    
    #: App/Fusers/accountprofil.ods.csv
    #, fuzzy
    msgid "CV_IUSER_ACCOUNT#label#EUSER"
    msgstr "Admin edit"
    
    #: App/Fusers/accountprofil.ods.csv
    #, fuzzy
    msgid "CV_IUSER_ACCOUNT#label#EGROUP"
    msgstr "Groupes"
    
    #: App/Fusers/accountprofil.ods.csv
    #, fuzzy
    msgid "CV_IUSER_ACCOUNT#menu#EGROUP"
    msgstr "Compte"
    
    #: App/Fusers/accountprofil.ods.csv
    #, fuzzy
    msgid "CV_IUSER_ACCOUNT#label#ESUBSTITUTE"
    msgstr "Modifier le suppléant"
    
    #: App/Fusers/accountprofil.ods.csv
    #, fuzzy
    msgid "CV_IUSER_ACCOUNT#menu#ESUBSTITUTE"
    msgstr "Compte"

Les clefs suivantes sont générées :

`CVNAME#label#IDVUE`
:   Libellé de la vue IDVUE du contrôle de vue CVNAME

`CVNAME#menu#IDVUE`
:   Libellé du menu de la vue IDVUE du contrôle de vue CVNAME

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
[cvdoc]:            #core-ref:017f061a-7c12-42f8-aa9b-276cf706e7e0
[buildtools]:       https://github.com/Anakeen/dynacase-buildtools   "Source BuildTools"