# Dcp\DocManager::getNameFromId()  {#core-ref:64654ded-b124-4573-a936-42f8605d20d7}

<div class="short-description">
Retourne l'identifiant associé au nom logique 
</div>


## Description  {#core-ref:737f3bf0-ab64-47d3-8c5e-020817fab370}

    [php]
    static string Dcp\DocManager::getNameFromId ( int $documentId )


Retourne le nom logique (propriété `name`) associé à l'identifiant numérique.

### Avertissements  {#core-ref:45cb0921-8861-4d50-a018-f823222dc1e5}

Aucun.

## Liste des paramètres  {#core-ref:a17be47a-1d8f-4877-ac48-99caf7e87bff}


(int) `documentId`
:   Identifiant numérique du document. Propriété (`id` ou `initid`).


## Valeur de retour  {#core-ref:db9718d3-eaee-4057-9c58-f1586cc7dc15}

Retourne le nom logique associé à l'identifiant numérique (propriété `id`).

## Erreurs / Exceptions  {#core-ref:a6665ce2-b307-472b-a74a-2c42c7f56870}

Retourne `null` si l'identifiant n'existe pas.

## Historique  {#core-ref:b2de2441-6b9f-40f8-866e-b929a13f36b7}

Remplace la fonction `getNameFromId`.

## Exemples  {#core-ref:f166ae9a-5342-46f2-81aa-1aff6a680930}

    [php]
    use AMyFamily as \Dcp\AttributeIdentifiers\MyFamily
    $docName=\Dcp\DocManager::getNameFromId(1234);
    if ($docName !== null) {
        printf("Nom logique : name = %s", $docName);
    }

## Notes  {#core-ref:77b499d4-6394-4084-8f49-e335ff3651ca}

Le même nom logique est associé à toutes les révisions d'un même document.

## Voir aussi  {#core-ref:a59db174-5120-4ef8-922c-2bb6632edecb}

*   [`Dcp\DocManager::getIdFromName()`][idfromname]

<!-- links -->

[idfromname]:   #core-ref:bd01446c-e759-4191-a538-a766359d7e33