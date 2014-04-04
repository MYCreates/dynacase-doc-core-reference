# Dcp\DocManager::cache() 
<div class="short-description">
Méthode d'accès au fonction de cache
</div>


## Description 

    [php]
    static Dcp\DocManager\Cache Dcp\DocManager::cache( )

Retourne l'objet "cache" utilisé par `DocManager`.

### Avertissements 

Aucun.

## Liste des paramètres 
 
Aucun.


## Valeur de retour 

Retourne l'objet "cache".

## Erreurs / Exceptions 

Aucune.

## Historique 

Aucun.

## Exemples 

Ajout d'un document :

    [php]
    $document=Dcp\DocManager::getDocument(1234);
    Dcp\DocManager::cache()->addDocument($document);

## Notes  

Aucune.

## Voir aussi 

*   [`Dcp\DocManager::getDocument()`][getdocument]
*   [`Dcp\DocManager\Cache::addDocument()`][adddocument]
*   [`Dcp\DocManager\Cache::hasDocument()`][hasDocument]

<!-- links -->
[getdocument]:      #core-ref:dfa0762f-6ff3-4349-bd21-6442740d9dcc
[hasDocument]:      #core-ref:45b26670-f06a-4054-959f-dc4408346e22
[adddocument]:      #core-ref:15d6a036-3b6e-4dbd-a0fe-361b925e6186