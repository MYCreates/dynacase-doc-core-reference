# `CORE_URLINDEX` {#core-ref:9081464e-dfc9-4836-8577-cfa59829eaa0}

## Description {#core-ref:330c7119-f87a-4c62-9e1e-d2d126c02e68}

Le paramètre `CORE_URLINDEX` permet de spécifier l'URL d'accès à l'index de
Dynacase. Cette URL est utilisée pour composer les URLs présentes sur les
interfaces HTML ou les [courriels][mailaction] émis par Dynacase.

*   App : `CORE`
*   Portée : Global
*   Valeur initiale : (vide)
*   Utilisateur : Non

## Valeurs {#core-ref:6a7603e5-92a7-45e3-906d-5f841d4fcae6}

La valeur du paramètre doit être une URL sous sa forme normalisée
(e.g. `http://www.example.net:8080/dynacase/`).

En l'absence de valeur, la valeur du paramètre est calculée à partir du nom
d'hôte du serveur.

## Notes {#core-ref:ad67b7d7-5bae-4df2-8553-1fac7e794d82}

L'URL ne doit pas contenir de requête (partie de la forme `?a=b&c=d`) ou de
hash (partie finale de la forme `#anchor`).

<!-- links -->

[mailaction]: #core-ref:c1d9e009-49a5-47a4-9104-4d044ea24aa3