# `FREEDOM_UPLOADDIR`   {#core-ref:8c1c8225-27d0-49a5-a092-81b6c433e20d}

## Description   {#core-ref:76f6b6a6-4e26-41cd-97e6-83916fb71664}

Ce paramètre indique le répertoire utilisé pour déposé les archives lors d'une
[importation][importarchive].

*   App : `CORE`
*   Portée : Global
*   Valeur initiale : `./var/tmp`
*   Utilisateur : Non

Par défaut, la valeur est identique au paramètre [`CORE_TMPDIR`][core_tmpdir].
Dans ce cas, les archives déposées sont supprimées comme les fichiers
temporaires.

Pour conserver les archives, il est nécessaire d'indiquer un autre répertoire
comme par exemple `./var/archive`.


## Valeur   {#core-ref:f3082f12-e557-4b2f-be6e-753f4cad900b}

La valeur doit désigner un répertoire accessible en écriture par le serveur web.

## Notes   {#core-ref:a9121831-7b50-45d9-9001-8caa84e2720f}

Si le répertoire est situé en dehors de l'espace d'installation, son contenu ne
sera pas récupéré lors de la constitution d'une [archive de contexte][savectx].

Lors de la restauration d'une archive de contexte, si la valeur de
`FREEDOM_UPLOADDIR` référence un répertoire à l'extérieur du répertoire du
contexte, alors sa valeur est réinitialisée à sa valeur initiale (i.e.
`./var/tmp`).

<!-- links -->

[importarchive]: #core-ref:021b7db1-7baf-48c4-8eb9-4a388355dd86
[core_tmpdir]: #core-ref:21893de9-7b24-49f2-a408-4fa2f8ca0951

[savectx]:    http://docs.anakeen.com/dynacase/3.2/dynacase-doc-platform-operating-manual/website/book/manex-ref:1ea676c8-8a00-4466-bd59-a86df03a32c4.html#manage-context-archivage "Archivage de contexte"
