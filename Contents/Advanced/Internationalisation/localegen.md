# Générer et mettre à jour les entrées du catalogue {#core-ref:2c163f00-8e94-4736-86f2-bb51352c52aa}

Le catalogue doit contenir des entrées pour toutes les clés à traduire.
Ces clés peuvent se trouver dans eds fichiers php, js, des templates, ainsi qu'à de nombreux autres endroits.
Ce chapitre décrit comment extraire toutes les chaînes à traduire, et produire le catalogue correspondant.

Dynacase requiert 2 catalogues par application :

-   Un catalogue pour la partie php.
    Il contient
    +   les traductions des fichiers php
    +   les traductions des workflows
    +   les traductions des familles de documents
    +   les traductions des contrôles de vue
    +   les traductions des templates
-   Un catalogue pour la partie javascript.
    Il contient
    +   les traductions des fichiers javascript

Plus de détails sur ces catalogues et leur déploiement peuvent être trouvés dans la partie
[Publication][core-ref:catalog_publication]

## Utilisation des devtools pour générer l'ensemble des catalogues {#core-ref:208450f2-9ea3-43af-bfa7-7e40cebe6d87}

Il est possible de générer les catalogues par le programme [`dynacase-devtool.phar`][devtoolphar].

    php dynacase-devtool.phar extractPo -s .

Voir le [`Quick Start`][quickpo] pour plus d'information.

## Génération manuelle {#core-ref:34932734-6c75-4503-90d1-6d05bad33988}

La génération du catalogue se fait en 2 étapes :

1.  extraction des clés de traduction depuis les sources (génère un fichier `*.pot`)
2.  mise à jour des fichiers `*.po` à partir du fichier `*.pot`

Les chapitres suivants détaillent comment extraire les clés de traduction à partir des sources.

<span class="flag inline nota-bene"></span> La mise à jour des fichiers `*.po` relève de l'utilisation de `gettext`
et n'est pas couverte par cette documentation.

<!-- links -->
[quickpo]:                      https://docs.anakeen.com/dynacase/3.2/dynacase-quick-start/website/book/quickstart:5b0e5ec5-1f8d-49ea-9953-42727cdc1b2b.html#quickstart:bec85337-36e8-4289-a938-f48b361e125e    "Générer les catalogues avec devTool"
[core-ref:catalog_publication]: #core-ref:7f5e9754-6db2-4dcb-ac99-e640f8a93c38