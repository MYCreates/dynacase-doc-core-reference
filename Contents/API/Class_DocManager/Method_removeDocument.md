# Dcp\DocManager\Cache::removeDocument()  {#core-ref:3ac7bd00-7420-4ce8-8bba-d5a86c2261f6}

<div class="short-description">
Enlève un document du cache
</div>


## Description  {#core-ref:5d86b069-425b-4fee-a048-054908564428}

    [php]
    static &Doc Dcp\DocManager\Cache::removeDocument ( Doc  &$document )


### Avertissements  {#core-ref:68d3a5ff-8dcd-4d9d-99d7-83cf581c81ac}


## Liste des paramètres  {#core-ref:989e4cde-84c0-466a-9f55-08d0ff3cc035}
 
(Dcp\Family\Document) `document`
:   Objet document à enlever du cache


## Valeur de retour  {#core-ref:037b31aa-6554-4059-83d2-505608767498}

Retourne l'objet qui a été enlevé du cache.
Retourne `null`, si l'objet n'est pas présent dans le cache.

## Erreurs / Exceptions  {#core-ref:524a79bd-76b0-4bb0-835c-6fc36a03f052}

Exception `\Dcp\DocManager\Exception`,  si le document n'a pas
d'identificateur.

## Historique  {#core-ref:2ff0e090-42f2-4ad2-a140-93c82d10d1b4}

Aucun.

## Exemples  {#core-ref:c295dfa1-e265-4c50-87c7-4343d29d6259}


    [php]
    $document=Dcp\DocManager::getDocument(1234);
    $myRemovedDocument=Dcp\DocManager::cache()->removeDocument($document);

## Notes  {#core-ref:d682b795-a6f9-4cda-abb3-c773d81fe6aa}


## Voir aussi  {#core-ref:495d8bc4-a206-49a9-a290-274f93931792}

*   [`Dcp\DocManager::getDocument`][getdocument]

<!-- links -->
[getdocument]:      #core-ref:dfa0762f-6ff3-4349-bd21-6442740d9dcc