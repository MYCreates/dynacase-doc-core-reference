# Dcp\DocManager::getRawValue()  {#core-ref:b718a637-9197-4b5e-9df2-34e8584d34af}

<div class="short-description">
Retourne la valeur brute d'une donnée de document.
</div>



## Description  {#core-ref:96d553d6-9624-4380-9710-71e6d67ee5ff}

    [php]
    static string getRawValue ( int|string $documentIdentifier,
                                    string $dataIdentifier
                                      bool $latest=true,
                                      bool $useCache=true )

Retourne la valeur brute d'un attribut ou d'une propriété du document.

### Avertissements  {#core-ref:91737e86-2285-4fa4-8094-12ee3c479bd5}

Cette méthode utilise le cache par défaut.

## Liste des paramètres  {#core-ref:8ef2099a-ca6d-42e8-8ce3-b5bea0dd546a}

(int|string) `documentIdentifier`
:   Identifiant numérique du document ou nom logique du document.

(string) `dataIdentifier`
:   Identifiant de l'attribut ou de la propriété.

(bool) `latest` 
:   Utilisable que uniquement pour les identifiants numériques. Indique si on veut la
    révision précise correspondant à l'identifiant ou la dernière révision.

(bool) `useCache`

:   Par défaut, la cache est activé. Cela signifie que la valeur est récupérée
    du cache s'il est présent dans celui-ci. Sinon, la valeur est 
    récupérée de la base de données.  
    Si `useCache` est mis à `false`, la valeur est systématiquement
    récupérée de la base de données et le cache n'est pas utilisé.

## Valeur de retour  {#core-ref:575cb8ff-e3c2-4d6c-8c64-b9a9fbc376e6}

La valeur **brute** de l'attribut. Toutes les valeurs retournées sont de type
`string`, même si l'attribut ou la propriété est de type `int`.

Retourne la valeur `null` si l'identifiant de document n'existe pas.

Retourne la valeur `""` (chaîne vide) si l'attribut est vide.

## Erreurs / Exceptions  {#core-ref:79769f3f-fedc-4568-bf52-d3131fd261e5}

Exception `\Dcp\DocManager\Exception`

*   Si l'identifiant n'est pas syntaxiquement valide. 
    L'identifiant doit être un nombre positif ou une chaîne
    de caractère non vide.
*   Si l'attribut n'existe pas 



## Historique  {#core-ref:e75a68bb-f5a4-4ae6-8800-ed75b4de4a34}

Remplace partiellement `getDocValue`.
Pas de parcours de relation.

## Exemples  {#core-ref:ff1efdcb-5ef4-4aa0-a37c-b407573fa480}

    [php]
    use AMyFamily as \Dcp\AttributeIdentifiers\MyFamily
    $rawNumber=\Dcp\DocManager::getRawValue(1234, AMyFamily::my_number1);
    
    

## Notes  {#core-ref:89615cd4-3efd-433b-bc34-fee5c8243219}

Cette méthode est plus rapide et consomme moins de mémoire que
`DocManager::getDocument()` suivi de [`Doc::getRawValue()`][getrawvalue].

Pas de vérification que l'identifiant est un attribut du document.
L'identifiant peut être toute donnée présente dans la base. Ceci inclus les
propriétés.

## Voir aussi  {#core-ref:1286e9bb-d516-40f3-b22a-89a6c62e992f}

*   [`Doc::getRawValue()`][getrawvalue]
*   [`DocManager\Cache::addDocument()`][addincache]

<!-- links -->

[getrawvalue]:      #core-ref:f779391c-ee61-4c3a-8976-6b74f83ecc8f
[addincache]:       #core-ref:15d6a036-3b6e-4dbd-a0fe-361b925e6186
