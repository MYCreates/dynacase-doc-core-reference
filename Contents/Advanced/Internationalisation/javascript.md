# Utiliser un catalogue dans les fonctions JavaScript {#core-ref:c5e3821a-4170-11e3-9b2b-b38f13ee44c4}

Le catalogue des traductions pour le javascript est fourni par un fichier "JSON".
Ce fichier est généré dans le répertoire :

    locale/<lang>/js/catalog.js

Par défaut les locales pour le français et l'anglais sont disponibles :

*   locale/fr/js/catalog.js
*   locale/en/js/catalog.js

Ce fichier JSON est un objet où les clefs de traduction sont les index.

Exemple :

    {
        "Hello" : "Bonjour",
        "The world" : "Le monde"
    }

<span class="flag from release">3.2.19</span> Les traductions avec contexte sont 
présentes dans le fichiers catalogue sous l'index `_msgctxt_`. Cet index contient
des sous-index par contexte.


Exemple : l'inspection du code javascript suivant :

    [javascript]
    var message=_("Hello")+_("World");
    var other=___("Hello","myFirstContext")+___("Today","myFirstContext");
    var alternative=___("Hello","mySecondContext")+___("Clear","mySecondContext");

fournira le catalogue suivant : 

    {
        "Hello" : "Bonjour",
        "The world" : "Le monde"
        "_msgctxt_" : {
            "myFirstContext" : {
                "Hello" : "Salut"
                "Today" : "Aujourd'hui"
            },
            "mySecondContext" : {
                "Hello" : "Bienvenue à tous"
                "Clear" : "Temps clair"
            }
        }
    }

Exemple de code javascript permettant d'utiliser le catalogue "JSON".

    [html]
    <html>
    <script type="text/javascript" src="lib/jquery/jquery.js"></script>
    <script>
        $(document).ready(function () {
            var catalog;
            $.getJSON("locale/fr/js/catalog.js", function (data) {
                catalog = data;
            });
            
            $("#translate").on("click", function () {
                var key = $("[name=key]").val();
                var ctx = $("[name=ctx]").val();
                var result;
                if (!ctx) {
                    result = catalog[key];
                } else if (catalog._msgctxt_[ctx]) {
                    result = catalog._msgctxt_[ctx][key]
                }
                if (!result) {
                    result = "Not found!"
                }
                $(".result").text(result);
            })
        });
    </script>
    <body>
        <input name="key" placeholder="key"/>
        <input name="ctx" placeholder="context"/>
        <button id="translate">Translate</button>
        <p class="result"> ... </p>
    </body>
    </html>

## Limitation {#core-ref:d9a3c4ac-aa89-43f4-8688-4e28759da201}

Les formes plurielles ne sont pas prises en compte.

## Génération du catalogue temporaire {#core-ref:54be89bc-4171-11e3-b408-cffb8e583c3}

Le programme [`xgettextJs`][buildtools] permet générer le catalogue temporaire.
<span class="flag from release">3.2.12</span>

    ./buildTools/xgettextJs --output=myCatalog.pot  myFile1.js myFile2.js
    
    myCatalog.pot wrote.

Ce catalogue temporaire peut ainsi être utilisé pour produire les fichiers
catalogue de traduction.

La nouvelle version de `xgettextJs` permet aussi d'extraire les 
formes avec contexte.<span class="flag from release">3.2.19</span>

Exemple :

    [javascript]
    function hello() {
        alert(_("Hello"));
        alert(___("Hello","myContext")); // New in 3.2.19
    }

## Publication du catalogue {#core-ref:5bd6f306-4171-11e3-999b-60d7dc830245}

Les catalogues sources (`.po`) doivent être publiés sur le serveur dans le
répertoire :

    locale/<lang>/js/src/

Les catalogues binaires (`.mo`) doivent être publiés sur le serveur dans le
répertoire :

    locale/<lang>/js/

Les catalogues sources sont prioritaires aux catalogues binaires. Cela implique
que s'il y a des clefs dupliquées c'est la traduction des fichiers sources qui
sera utilisée.

Le répertoire `<lang>` est l'identifiant de la locale sur deux lettres : 

-   `fr` : français
-   `en` : anglais

Lors de la publication, ces fichiers `.po` servent d'entrées pour la production
du catalogue JSON en utilisant le programme `programs/po2js`. 
Plus de détails sur le paragraphe [Publication][popublish].



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
[popublish]:        #core-ref:7f5e9754-6db2-4dcb-ac99-e640f8a93c38 "Publication des catalogues"
[buildtools]:       https://github.com/Anakeen/dynacase-buildtools  "Source BuildTools"
