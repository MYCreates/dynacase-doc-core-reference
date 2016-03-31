# Extraction des clés pour les fichiers javascript {#core-ref:54be89bc-4171-11e3-b408-cffb8e583c3}

La génération des catalogues est effectuée traditionnellement par le programme
 [xgettext][xgettext].

    xgettext --language=JavaScript \
             --sort-output \
             --from-code=utf-8 \
             --no-location \
             --indent \
             --add-comments=_COMMENT \
             --keyword=___:1 \
             --keyword=___:1,2c \
             --keyword=n___:1,2 \
             --keyword=pgettext:1c,2 \
             --keyword=n___:1,2,4c \
             --keyword=npgettext:1,2,4c \
             --keyword="N_" \
             --keyword="text" \
             --keyword="Text" \
             myFile1.js myFile2.js ...

Cette fonction produit un catalogue de traduction temporaire permettant de
servir de base pour réaliser les traductions.

## Utilisation de `xgettextJs` {#core-ref:80338442-776f-4423-bbe7-0259b91703b9}

Le programme [`xgettextJs`][buildtools] permet générer le catalogue temporaire.
<span class="flag from release">3.2.12</span>

    ./buildTools/xgettextJs --output=myCatalog.pot  myFile1.js myFile2.js
    
    myCatalog.pot wrote.

La nouvelle version de `xgettextJs` permet aussi d'extraire les 
formes avec contexte.<span class="flag from release">3.2.19</span>

<!-- links -->
[buildtools]:   https://github.com/Anakeen/dynacase-buildtools  "Source BuildTools"
