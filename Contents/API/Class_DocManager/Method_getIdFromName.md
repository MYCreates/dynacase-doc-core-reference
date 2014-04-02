# Dcp\DocManager::getIdFromName()  {#core-ref:bd01446c-e759-4191-a538-a766359d7e33}

<div class="short-description">
Retourne l'identifiant associé au nom logique 
</div>


## Description  {#core-ref:32b53d71-8f6e-4f8e-9537-e3ff7f9f9712}

    [php]
    static int Dcp\DocManager::getIdFromName ( string $documentName )


Retourne l'identifiant numérique associé au nom logique.

### Avertissements  {#core-ref:95fa1106-dd9c-4fa5-8c8e-83f6066eb6a9}

Aucun.

## Liste des paramètres  {#core-ref:67c96949-7e3b-4473-a8b9-1b4d7513b25a}


(string) `documentName`
:   Nom logique du document


## Valeur de retour  {#core-ref:093bf2cf-26ea-408e-8391-1831fd371770}

Retourne l'identifiant de la dernière révision de document ayant le nom
logique indiqué.

## Erreurs / Exceptions  {#core-ref:cbbb4034-4a46-4bd5-8a0f-df1499790907}

Retourne `null` si le nom logique n'existe pas.

## Historique  {#core-ref:39839012-139f-4f41-9006-da506bb94c81}

Remplace la fonction `getIdFromName`.

## Exemples  {#core-ref:9c476406-d064-40fe-8691-21b63c775e62}

    [php]
    use AMyFamily as \Dcp\AttributeIdentifiers\MyFamily
    $docId=\Dcp\DocManager::getIdFromName('MY_DOCUMENT');
    if ($docId !== null) {
        printf("Id %d", $docId);
    }

## Notes  {#core-ref:3e4a9be1-eca1-49b8-a9e0-9946127a2659}

Aucun.

## Voir aussi  {#core-ref:26edb16c-1731-49de-b590-7190f3757d81}

*   [`Dcp\DocManager::getNameFromId()`][namefromid]

<!-- links -->

[namefromid]:       #core-ref:64654ded-b124-4573-a936-42f8605d20d7