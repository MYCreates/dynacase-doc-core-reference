# `CORE_TMPDIR`  {#core-ref:21893de9-7b24-49f2-a408-4fa2f8ca0951}

## Description  {#core-ref:d13ad60f-4a5f-4e5b-8cf7-a2efd65e3ca6}

Ce paramètre indique le répertoire temporaire utilisé par Dynacase.

*   App : `CORE`
*   Portée : Global
*   Valeur initiale : `./var/tmp`
*   Utilisateur : Non

Les fichiers de ce répertoire sont supprimés de façon régulière par le script
[`cleanContext`][wsh_cleanContext].

## Valeur  {#core-ref:4b61cb82-3dc3-4ec8-b501-d390dd861241}

La valeur doit désigner un répertoire accessible en écriture par le serveur web.

## Notes  {#core-ref:bbd196fb-b9c4-4ec4-9280-edcab1e0886b}

Voir aussi [`CORE_TMPDIR_MAXAGE`][core_tmpdirmaxage].

Lors de la restauration d'une archive de contexte, si la valeur de
`CORE_TMPDIR` référence un répertoire à l'extérieur du répertoire du contexte,
alors sa valeur est réinitialisée à sa valeur initiale (i.e. `./var/tmp`).

<!-- links -->

[wsh_cleanContext]: #core-ref:100b123b-da1a-45b4-848b-0622f3e09a40
[core_tmpdirmaxage]: #core-ref:24a36fc9-b3bb-4bab-a06c-ac28c4372d57
