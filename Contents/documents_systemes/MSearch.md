# Recherche groupée {#core-ref:fee105b5-9cd8-42db-8456-ed39aafdf086}

## Présentation {#core-ref:a465b43e-58b4-4b3e-8afa-1c147cf5c4a1}

Une recherche groupée est une union (au sens ensembliste du terme) de plusieurs
recherches (au niveau de la base de données, c'est également la construction
[UNION][psql_union] qui est utilisée).

Elle est implémentée au moyen de la famille `MSEARCH`, qui étend la famille
`SEARCH`.

La recherche groupée n'exécute que les documents "recherche" qui ont le
[droit][profil] "execute" pour l'utilisateur courant.

## Éléments de paramétrage {#core-ref:e12621a9-0884-425b-9517-71494411ce5c}

Lors de la construction d'une recherche groupée, il suffit de spécifier la liste
des recherches à utiliser. La recherche groupée est ensuite utilisable de façon
identique à la recherche simple.

<!-- links -->
[psql_union]: http://www.postgresql.org/docs/9.3/static/queries-union.html "Documentation officielle de la construction UNION"
[profil]:   #core-ref:a99dcc5f-f42f-4574-bbfa-d7bb0573c95d