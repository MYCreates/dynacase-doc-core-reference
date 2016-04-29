# Les Hooks {#core-ref:8f3d47de-32b5-4748-8a00-b1569c5423e5}

Le document offre de nombreux [hooks][WP_hooks] permettant de modifier son 
comportement au cours des étapes telles que la création, modification, 
suppression, etc. de document.

Ces hooks viennent s'ajouter aux droits de l'utilisateur. Par exemple, lors de
la création d'un document, Dynacase vérifiera d'abord que l'utilisateur a bien
le droit de créer un document de la famille concernée, puis vérifiera ensuite
que le hook [`Doc::preCreated()`][doc_precreated] ne bloque pas cette création.

## Création de document {#core-ref:b28f8dcb-c0a3-4df5-b3e5-4f55611d3a4d}

Les méthode surchargeables appelées lors de la création d'un document sont :

*   Lors de la création avec [`Doc::store()`][doc_store]
    *   [`Doc::preStore()`][doc_prestore]
    *   [`Doc::preCreated()`][doc_precreated]
    *   [`Doc::customSearchValues()`][doc_customSearchValues]
    *   [`Doc::postCreated()`][doc_postcreated]
    *   [`Doc::preRefresh()`][doc_prerefresh]
    *   [`Doc::postRefresh()`][doc_postrefresh]
    *   [`Doc::postStore()`][doc_poststore]
*   Lors de la création avec [`Doc::add()`][dbobjadd]
    *   [`Doc::preCreated()`][doc_precreated]
    *   [`Doc::postCreated()`][doc_postcreated]

## Modification de document {#core-ref:b09fd0c9-d935-4db2-b942-8a80a4bffaf3}

Les méthode surchargeables appelées lors de la modification d'un document sont :

*   Lors de la modification avec [`Doc::store()`][doc_store]
    *   [`Doc::preStore()`][doc_prestore]
    *   [`Doc::customSearchValues()`][doc_customSearchValues]
    *   [`Doc::preRefresh()`][doc_prerefresh]
    *   ([`Doc::customSearchValues()`][doc_customSearchValues] si modification)
    *   [`Doc::postRefresh()`][doc_postrefresh]
    *   [`Doc::postStore()`][doc_poststore]
*   Lors de la modification avec `Doc::modify()`
    *   aucune

## Suppression de document {#core-ref:10c4622a-8834-4b9b-94ba-93f278d9bde0}

Les méthode surchargeables appelées lors de la suppression d'un document 
avec [`Doc::delete()`][doc_delete] sont :

*   [`Doc::preDelete()`][doc_predelete]
*   [`Doc::postDelete()`][doc_postdelete]

## Affectation de document {#core-ref:e11b3532-6d5b-4a1a-ad20-0667409f1f65}

<span class="flag from release inline">3.2.20</span>
Les méthode surchargeables appelées lors de l'affectation d'un document sont

*   [`Doc::preAffect()`][doc_preaffect]
*   [`Doc::postAffect()`][doc_postaffect]

utilisé principalement par la fonction [`new_doc()`][new_doc] et par la classe 
[`SearchDoc`][searchdoc].

## Duplication de document {#core-ref:db739471-d1e7-4762-9cfb-1de1a823b06a}

Les méthode surchargeables appelées lors de la duplication d'un document 
avec [`Doc::duplicate()`][doc_duplicate]

*   `Doc::preDuplicate()`
*   `Doc::postDuplicate()`

## Import de document {#core-ref:a95a3a83-fab4-455c-8cb4-9b278f770f3a}

Les méthode surchargeables appelées lors de l'import d'un document sont :

*   [`Doc::preImport()`][doc_preimport]
*   [`Doc::postImport()`][doc_postimport]

## Ajout d'un document dans un dossier {#core-ref:05847fea-7a2f-4b67-8b4e-ef56d225fcec}

Les méthode surchargeables appelées lors de l'ajout d'un document
dans un dossier sont :

*   Lors de l'insertion avec [`Dir::insertDocument()`][dir_insertdocument]
    *   `Dir::preInsertDocument()`
    *   `Dir::postInsertDocument()`
*   Lors de l'insertion avec [`Dir::insertMultipleDocuments()`][dir_insertmultiple]
    *   `Dir::preInsertMultipleDocuments()`
    *   `Dir::postInsertMultipleDocuments()`

## Retrait d'un document d'un dossier {#core-ref:a0ec9634-095a-4caa-94e5-7a39d141e185}

Les méthode surchargeables appelées lors du retrait d'un document
dans un dossier avec [`Dir::removeDocument()`][dir_removedocument] sont :

*   `Dir::preRemoveDocument()`
*   `Dir::postRemoveDocument()`

<!-- links -->
[advanced_dbobj]: #core-ref:7a62bb83-17a0-478d-a853-bc359d0fb8fb
[WP_hooks]: http://fr.wikipedia.org/wiki/Hook_%28informatique%29 "Définition des hooks sur wikipedia"
[doc_store]: #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[doc_prestore]: #core-ref:3517da95-82fe-4adb-8bc4-ef49ca55edb0
[doc_precreated]: #core-ref:e85aa9d4-5e62-4a60-9d1c-f60433301747
[doc_postcreated]: #core-ref:b8f80e6b-a374-4bf4-bc76-47290cd69c45
[doc_prerefresh]: #core-ref:580d6be1-6b6a-439b-abd7-34b26cfaf2e5
[doc_postrefresh]: #core-ref:9352c534-3691-41e3-b293-599db8e9a4fd
[doc_poststore]: #core-ref:99520a31-0aef-4bc6-b20a-114737059d17
[doc_delete]:   #core-ref:c4372b13-c132-4148-9487-de2b7614d497
[doc_duplicate]:   #core-ref:f7d4f454-0e45-40bd-9f4c-b149ab620295
[doc_preimport]: #core-ref:adb6ba8b-15c4-42d3-97dc-1da16c2112ae
[doc_postimport]: #core-ref:9de7e922-150a-416b-b846-b6e195bf0921
[doc_predelete]: #core-ref:dca7b2bd-fc69-4f6f-ab12-fb0de91dce8c
[doc_postdelete]: #core-ref:8db8e5e0-b15e-4cfe-a891-66850df15b1e

[doc_preaffect]:        #core-ref:e6f36fea-9f42-4751-ba9b-c3aafec56559
[doc_postaffect]:       #core-ref:7e9f3b6f-f801-4fa9-8215-f02d575b357f
[dir_insertdocument]:   #core-ref:9575ff95-480a-4dfb-9cd0-b89f44c3fad7
[dir_removedocument]:   #core-ref:d337e186-8066-49e2-92a0-26aa518cbf41
[dir_insertmultiple]:   #core-ref:098cf44e-568d-4dd2-8dd0-e2f104bc8615
[dbobjadd]:             #core-ref:28379dfc-7f6d-450f-b994-834d4fba7452
[new_doc]:              #core-ref:e978cbd1-5f54-4a06-a6be-f1c079c2d734
[searchdoc]:            #core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255
[doc_customSearchValues]: #core-ref:f1aaac21-085c-4ef6-bddc-962530c4efaa