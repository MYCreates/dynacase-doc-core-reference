# Lignée documentaire et révisions {#core-ref:1cdff481-42e0-4caf-baba-d2348d760ca5}

Afin de manipuler correctement les documents, il est important de comprendre
les notions de *révision* et de *lignée documentaire*.

## Révision {#core-ref:8c947ef6-8da5-4fca-93cf-897c81ebb53d}

Une révision est une capture d'un document à un instant donné. Les valeurs
(attributs, propriétés et fichiers) de ce document sont mémorisées et figées, et
peuvent être consultées ultérieurement.

À un instant donné, une et une seule révision est *active* (c'est la révision
courante) alors que les autres sont figées (ce sont les révisions passées).

Seule la révision *active* est modifiable, les révisions *figées* sont uniquement
consultables.

## Lignée documentaire {#core-ref:ce7f84b8-9ce4-404f-b200-8f1d5ecc9808}

L'ensemble des révisions d'un document constituent une lignée documentaire.
Alors que chaque document est identifié de manière non ambigüe par son `id`,
unique, une lignée documentaire est identifiée par son `initid`.

## `révision`, `id` et `initid` {#core-ref:92242e66-ef53-45ff-b565-99857b28c08b}

Lors de l'enregistrement en base d'un document, s'il ne possède pas encore d'`id`,
un nouvel `id` est attribuée au document. Cette valeur est également
affectée à la propriété `initid`. La propriété révision, quand à elle, est
positionnée à 0.

Lorsqu'un document est révisé, techniquement, un nouveau document est créé
(c'est à dire qu'une nouvelle ligne est créée en base de données).  
La propriété révision est incrémentée, la propriété `initid` est conservée, et un
nouvel `id` est affecté au document. L'ancienne révision est également marqué
comme figée (voir la propriété [`locked`][locked]).

Exemple :

-   création d'un nouveau document
    -   ancienne révision
        -   aucune
    -   nouvelle révision
        -   initid : 1234
        -   id : 1234
        -   revision : 0
        -   locked : 0
-   révision du document
    -   ancienne révision
        -   initid : 1234
        -   id : 1234
        -   revision : 0
        -   locked : -1
    -   nouvelle révision
        -   initid : 1234
        -   id : 2345
        -   revision : 1
        -   locked : 0

Note : les valeurs des `id` et `initid` sont arbitraires, elles sont, dans les
faits, affectées par la base de données.

## Révisions et relations {#core-ref:62b426a0-16b5-4adc-9ddc-0f5601859d95}

Dans l'interface d'édition, par défaut, les attributs de type [docid][docid]
sont valués avec l'`initid` du document cible. Lors de la consultation, le lien
est ajusté pour pointer vers la dernière révision de sa lignée documentaire.

Ce comportement peut être modifié au moyen de l'option [`docrev` des attributs
relation][docid].

## Révisions et `new_doc` {#core-ref:4932c928-7789-4bd2-8493-aa781caf21a9}

Par défaut, la fonction [`new_doc`][newdoc] récupère la révision correspondant à
l'`id` passé en paramètre ; ce qui veut dire que ce n'est pas nécessairement la
révision courante. Le troisième paramètre de `new_doc` permet de récupérer
systématiquement la révision courante.

Si l'identifiant de la fonction `new_doc` désigne un nom logique, c'est toujours
la version courante qui est retournée. 

<span class="flag inline nota-bene"/> Le nom logique est le même sur toute la 
lignée documentaire.

## Révisions et recherche {#core-ref:0164de5f-5968-4a43-8963-30a087314a83}

Lors des recherches de documents avec un critère sur une relation, si la
relation ne contient pas l'`initid`, on risque de ne pas trouver le document.
C'est pour cette raison que les relations contiennent l'`initid` par défaut. Dans
le cas où d'autres révisions peuvent être référencées, il faut ajouter un
critère plus complexe tenant compte des ids de toutes les révisions.

<!-- links -->

[locked]: #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353
[docid]: #core-ref:d461d5f5-b635-47a0-944d-473c227587ab
[newdoc]:  #core-ref:e978cbd1-5f54-4a06-a6be-f1c079c2d734
