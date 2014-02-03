# `CORE_DBDOCVIEWCOMPAT` 

## Description 

Le paramètre `CORE_DBDOCVIEWCOMPAT` permet de conserver l'usage des tables de
documents telles qu'elles étaient nommée dans la version 9.2 de Dynacase.

*   App : `CORE`
*   Portée : Global
*   Valeur possible : `no` ou `yes`
*   Valeur initiale : `no`
*   Utilisateur : Non

## Valeur 

Si la valeur est `yes`, des vues de compatibilités sont construites lors de la
[génération des familles][generate].

Exemple pour la famille `DIR` : ajout de la vue `doc2`

    [sql]
    create view public.doc2 as (select * from family.dir)

Les vues `public.docfam` et `public.doc` sont aussi ajoutées et pointent
respectivement sur les tables [`family.families`][docfam] et
[`family.documents`][dbdoc].

Si la valeur est `no`, aucune vue de compatibilité est ajoutée.


## Notes 

La valeur par défaut est `no`.

<!-- links -->
[generate]: #core-ref:8566efe6-782a-4c13-8117-f1ad99a7ad02
[docfam]:  #core-ref:d4b8d8ce-6f7a-4c1c-a5c4-f1adfcb74864
[dbdoc]:  #core-ref:0c6cc474-d5e9-4ee0-aeed-1aa00100d7df
