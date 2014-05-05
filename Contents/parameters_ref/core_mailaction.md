# `CORE_MAILACTION` {#core-ref:c1d9e009-49a5-47a4-9104-4d044ea24aa3}

## Description {#core-ref:15b17075-0429-4e50-8a22-b37e8c6e6f69}

Le paramètre `CORE_MAILACTION` permet de spécifier l'URL d'accès à une
application et une action utilisée pour les liens dans les mails émis par
Dynacase.

*   App : `CORE`
*   Portée : Global
*   Valeur initiale : (vide)
*   Utilisateur : Non

## Valeurs {#core-ref:1a7d170d-e801-4095-881a-b9e9d9be4de4}

La valeur du paramètre doit être une URL, avec une requête, sous sa forme
normalisée (e.g.
`http://www.example.net:8080/dynacase/?app=MYAPP&action=MYACTION`).

En l'absence de valeur, la valeur du paramètre sera calculée à partir de la
valeur du paramètre [CORE_URLINDEX][core_urlindex].

## Notes {#core-ref:516b9485-981e-44f2-ad69-f4a1b2cf3277}

L'URL doit contenir une chaîne de requête qui référence une application et une
action Dynacase valide (e.g. `?app=MYAPP&action=MYACTION`).

L'id du document est ajoutée à la suite du paramètre. Aussi, si vous utilisez
un *hash* (partie finale de la forme `#anchor`), l'action n'aura pas accès à
cet id. Cela peut être utile dans le cas ou le routage est fait coté client.

<!-- links -->
[core_urlindex]: #core-ref:9081464e-dfc9-4836-8577-cfa59829eaa0