# Extraction des clés pour les fichiers PHP {#core-ref:6feaa982-4171-11e3-9e01-48f953959281}

La génération des catalogues est effectuée traditionnellement par le programme
 [xgettext][xgettext].

    xgettext --language=PHP \
             --keyword=___:1 \
             --keyword=___:1,2c \
             --keyword=n___:1,2 \
             --keyword=pgettext:1,2 \
             --keyword=n___:1,2,4c \
             --keyword=npgettext:1,2,4c \
             --keyword='N_'  \
             --keyword='text'  \
             --keyword='Text' \
             --from-code=utf-8  \
             --output=myCatalog.pot \
             myFile1.php myFile2.php ...

Cette fonction produit un catalogue de traduction temporaire permettant de
servir de base pour réaliser les traductions.

## Utilisation de `xgettextPhp` {#core-ref:331ad226-1971-49c1-9116-099d2f25d96b}

Le programme [`xgettextPhp`][buildtools] permet de réaliser la même fonction et récupère en
plus les libellés des [fonctions de recherche][searchLabel].
<span class="flag from release">3.2.12</span>

    ./buildTools/xgettextPhp --output=myCatalog.pot  myFile1.php myFile2.php
    
    myCatalog.pot wrote.

Les options de `xgettext` sont utilisables pour modifier le fichier produit.

    ./buildtools/xgettextPhp --omit-header --add-location -o myLayouts.pot test.html

Il est possible de déclarer les fichiers d'entrées via le pipe.

    find . -name "*php" | ../buildtools/xgettextPhp --output myCatalog.pot -f-

<!-- link -->
[buildtools]:       https://github.com/Anakeen/dynacase-buildtools   "Source BuildTools"
