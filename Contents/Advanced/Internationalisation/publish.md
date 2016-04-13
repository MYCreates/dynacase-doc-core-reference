# Publier et mettre à jour les catalogues {#core-ref:7f5e9754-6db2-4dcb-ac99-e640f8a93c38}

Les catalogues doivent être déposés dans des sous-répertoires de

    locale/<lang>/

Le répertoire `<lang>` est l'identifiant de la locale sur deux lettres :

-   `fr` : français
-   `en` : anglais
-   …

## Catalogues php {#core-ref:a29af85b-d6fc-4d52-b608-4b1524218a07}

Les catalogues sources (`.po`) doivent être publiés sur le serveur dans le
répertoire :<span class="flag from release">3.2.12</span>

    locale/<lang>/LC_MESSAGES/src/

Les catalogues binaires (`.mo`) doivent être publiés sur le serveur dans le
répertoire :

    locale/<lang>/LC_MESSAGES/

Les catalogues sources sont prioritaires aux catalogues binaires. Cela implique
que s'il y a des clefs dupliquées, c'est la traduction des fichiers sources qui
est utilisée.

## Catalogues javascript {#core-ref:5bd6f306-4171-11e3-999b-60d7dc830245}

Les catalogues sources (`.po`) doivent être publiés sur le serveur dans le
répertoire :

    locale/<lang>/js/src/

Les catalogues binaires (`.mo`) doivent être publiés sur le serveur dans le
répertoire :

    locale/<lang>/js/

Les catalogues sources sont prioritaires aux catalogues binaires. Cela implique
que s'il y a des clefs dupliquées c'est la traduction des fichiers sources qui
sera utilisée.

### Catalogues javascript au format JSON {#core-ref:6cbb5452-67c5-4bd1-a013-280dc2ad9e40}

Lors de la publication, ces fichiers `.po` servent d'entrées pour la production
du catalogue JSON en utilisant le programme `programs/po2js`.

Le fichier JSON correspondant est un objet où les clefs de traduction sont les index.

Exemple :

    {
        "Hello" : "Bonjour",
        "The world" : "Le monde"
    }

<span class="flag from release">3.2.19</span> Les traductions avec contexte sont 
présentes dans le fichiers catalogue sous l'index `_msgctxt_`. Cet index contient
des sous-index par contexte.

Exemple : le catalogue correspondant au code javascript suivant :

    [javascript]
    var message=_("Hello")+_("World");
    var other=___("Hello","myFirstContext")+___("Today","myFirstContext");
    var alternative=___("Hello","mySecondContext")+___("Clear","mySecondContext");

est :

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

## Prise en compte des nouveaux catalogues {#core-ref:8f5fb4ae-2206-4894-a258-d392470d3589}

Pour prendre en compte les nouvelles traductions, il faut lancer le programme :

    $ ./whattext

sur le serveur depuis le répertoire d'installation.

Ce programme fusionne l'ensemble des catalogues dans un catalogue principal.
C'est ce catalogue qui est chargé lors de l'exécution des actions et scripts Dynacase.

Ce programme va également générer les catalogues au format JSON en utilisant le programme `programs/po2js`.
Ce sont ces catalogues qui sont [utilisables dans le code javascript][translate_js].

## Header {#core-ref:a59b5c3f-502d-4679-8197-7654b4e2a5bb}

Lors de la génération du catalogue principal, c'est l'entête utilisée
dans le fichier `header.po` qui défini l'entête et donc la forme plurielle
utilisée.

Ce fichier définit notamment les formes plurielles au moyen de l'entête `Plural-Forms`

    msgid ""
    msgstr ""
    "Project-Id-Version: Test 1.0\n"
    "POT-Creation-Date: 2013-10-24 17:28+0200\n"
    "PO-Revision-Date: 2013-10-25 08:53+0200\n"
    "Last-Translator: FULL NAME <EMAIL@ADDRESS>\n"
    "Language-Team: LANGUAGE <LL@li.org>\n"
    "Language: French\n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"
    "Plural-Forms: nplurals=2; plural=n>1;\n"

Pour un même langage, la définition des formes plurielles de tous les catalogues
doivent être identique. Un catalogue français ne peut donc avoir deux plurielles
alors qu'un autre en défini trois.

Pour le français la définition de la forme plurielle doit être :

    "Plural-Forms: nplurals=2; plural=n>1;\n"

Pour l'anglais la définition de la forme plurielle doit être :

    "Plural-Forms: nplurals=2; plural=n != 1;\n"

## Remarques {#core-ref:4306c58b-31cc-461c-aa1a-8a6072f89298}

Note : les catalogues binaires peuvent être obtenus par le programme `msgfmt` et
inversement les catalogues sources peuvent être obtenus par le programme
`msgunfmt`.

<!-- link -->
[translate_js]:    #ddui-ref:0a0ae356-3e7f-439c-8fac-d5dcd5fc6767