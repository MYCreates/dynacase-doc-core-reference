# Dcp\DocManager\Cache::addDocument()  {#core-ref:15d6a036-3b6e-4dbd-a0fe-361b925e6186}

<div class="short-description">
Ajoute un document dans le cache
</div>


## Description  {#core-ref:0b14ba55-0939-47a6-9944-e02e871a4500}

    [php]
    static &Doc Dcp\DocManager\Cache::addDocument( Doc &$document )

Ajoute un document en cache. Si le document (même identifiant) est déjà présent
dans le cache, alors le cache est mis à jour.

### Avertissements  {#core-ref:2185f920-4dda-4ab4-9a55-a25eeffa60f3}

Aucun.

## Liste des paramètres  {#core-ref:e835fa37-cec4-4376-b2b0-8fac2a6c5ea5}
 
(Doc) `document`
:   Objet document à mettre en cache


## Valeur de retour  {#core-ref:e39c453e-0308-41d5-8439-8ab08e9c078b}

Retourne l'objet mis en cache.

## Erreurs / Exceptions  {#core-ref:df2d93d2-5e24-4070-8733-f42a0f01708d}

Exception `\Dcp\DocManager\Exception`,  si le document n'a pas
d'identificateur et s'il ne possède pas toutes les caractéristiques d'un document
(ensemble des propriétés) instancié.

## Historique  {#core-ref:0a3691ef-c8d5-4e7e-8341-4aa51220fed1}

Aucun.

## Exemples  {#core-ref:026c0d10-eea8-4dfb-83f4-f360157c7078}

    [php]
    $document=Dcp\DocManager::getDocument(1234);
    Dcp\DocManager::cache()->addDocument($document);

## Notes  {#core-ref:25483f2d-8036-4add-b04b-f568e6e8c07b}

Aucune.

## Voir aussi  {#core-ref:164a9afe-5033-41db-ab27-07b749a12b1d}


*   [`Dcp\DocManager::getDocument()`][getdocument]
*   [`Dcp\DocManager\Cache::hasDocument()`][isincache]

<!-- links -->
[getdocument]:      #core-ref:dfa0762f-6ff3-4349-bd21-6442740d9dcc
[isincache]:        #core-ref:45b26670-f06a-4054-959f-dc4408346e22