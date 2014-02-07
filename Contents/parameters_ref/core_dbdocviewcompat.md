# `CORE_DBDOCVIEWCOMPAT`  {#core-ref:7bb6122b-ab0e-4d9f-8a67-2643d2369aa8}

## Description  {#core-ref:c28e18d6-2d56-4946-9717-d74cf4fc9021}

Le paramètre `CORE_DBDOCVIEWCOMPAT` permet de conserver l'usage des tables de
documents telles qu'elles étaient nommées dans les versions 3.2 et précédentes de Dynacase.

*   App : `CORE`
*   Portée : Global
*   Valeur possible : `no` ou `yes`
*   Valeur initiale : `no`
*   Utilisateur : Non

## Valeur  {#core-ref:d1faf1db-e5ac-48b9-8e05-a624176c51c5}

Si la valeur est `yes`, des vues de compatibilités sont construites lors de la
[génération des familles][generate].

Exemple pour la famille `DIR` : ajout de la vue `doc2`

    [sql]
    create view public.doc2 as (select * from family.dir)

Les vues `public.docfam` et `public.doc` sont aussi ajoutées et pointent
respectivement sur les tables [`family.families`][docfam] et
[`family.documents`][dbdoc].

Si la valeur est `no`, aucune vue de compatibilité n'est ajoutée.


## Notes  {#core-ref:b5982d9a-b660-4323-ab62-813969d775f2}

La valeur par défaut est `no`.

<!-- links -->
[generate]: #core-ref:8566efe6-782a-4c13-8117-f1ad99a7ad02
[docfam]:  #core-ref:d4b8d8ce-6f7a-4c1c-a5c4-f1adfcb74864
[dbdoc]:  #core-ref:0c6cc474-d5e9-4ee0-aeed-1aa00100d7df
