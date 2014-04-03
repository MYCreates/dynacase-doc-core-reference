# Dcp\DocManager::getDocument()  {#core-ref:dfa0762f-6ff3-4349-bd21-6442740d9dcc}

<div class="short-description">
Retourne un document en fonction de son identifiant.
</div>

## Description  {#core-ref:e187519b-961d-41e6-b247-899c91984376}

    [php]
    static Dcp\Family\Document getDocument ( int|string $documentIdentifier,
                                                   bool $latest=true,
                                                   bool $useCache=true )

Retourne un objet [_Document_][DocClass] en fonction de son identifiant et de sa
famille.

### Avertissements  {#core-ref:1f7a0159-f559-4243-8345-37d92d27768d}

Les droits d'accès ne sont pas vérifiés.

## Liste des paramètres  {#core-ref:95c4959b-cfc0-400f-a622-6592e63bfaeb}

(int|string) `documentIdentifier`
:   Identifiant numérique du document ou nom logique du document.

(bool) `latest` 

:   Utile uniquement pour les identifiants numériques. Indique si on veut la
    révision correspondant à l'identifiant ou la dernière révision.

(bool) `useCache`

:   Par défaut, la cache est activé. Cela signifie que le document est récupéré
    du cache s'il est présent dans celui-ci. Sinon, les données du document sont 
    récupérées de la base de données.  
    Si `useCache` est mis à `false`, les données du document sont systématiquement
    récupérées de la base de données et le cache n'est pas utilisé.  
    Ce paramètre n'indique pas que le document est mis en cache 


## Valeur de retour  {#core-ref:630efb7b-d811-48de-aa05-e8bdc5303751}

Retourne un objet de la classe de la famille qui correspond au document.

Si l'identifiant ne correspond à aucun document `null` est retourné.

Retourne la dernière révision correspondante du document par défaut.


## Erreurs / Exceptions  {#core-ref:c90f798d-6682-4e1c-8e60-21b2204a586d}

Exception `\Dcp\DocManager\Exception`, si l'identifiant n'est pas
syntaxiquement valide. L'identifiant doit être un nombre positif ou une chaine
de caractère non vide.

## Historique  {#core-ref:b4927607-0bb7-41a0-be04-83dee7645f9c}

Remplace la fonction [`new_Doc`][new_doc].

## Exemples  {#core-ref:db198f05-cd82-44b5-a356-34af8085ff2c}

### Accès par identifiant {#core-ref:2f87988e-90d8-4497-9a33-1de9f8eec316}

Identifiant numérique :

    [php]
    $doc=\Dcp\DocManager::getDocument(1234);
    if ($doc !== null && $doc->isAlive()) {
        if ($doc->hasPermission('view')) {
            print $doc->getTitle();
        }
    }

Nom logique :

    [php]
    $doc=\Dcp\DocManager::getDocument('MY_DOCUMENT');
    if ($doc !== null && $doc->isAlive()) {
        if ($doc->hasPermission('view')) {
            print $doc->getTitle();
        }
    }


### Utilisation du cache {#core-ref:59ec66af-1f4f-4ac0-8da1-90f1e0846a3d}

    [php]
    use AMyFamily as \Dcp\AttributeIdentifiers\MyFamily
    function myFirst($docid) {
        $doc=\Dcp\DocManager::getDocument($docid);
        if ($doc !== null && $doc->isAlive()) {
            // ici on met le document en cache
            Dcp\DocManager::cache->addDocument($doc); 
            $doc->setAttributeValue(AMyFamily::my_number1, 324)
        }
    }
    function mySecond($docid) {
        // ici on récupère le document du cache
        $doc=\Dcp\DocManager::getDocument($docid); 
        if ($doc !== null && $doc->isAlive()) {
            $doc->setAttributeValue(AMyFamily::my_number2, 765)
        }
    }
    function myFinal() {
        myFirst(1234); // le document est mis en cache par cette méthode
        mySecond(1234);
        // ici on récupère le document du cache
        $doc=\Dcp\DocManager::getDocument(1234);
        if ($doc !== null && $doc->isAlive()) {
            $n1=$doc->getAttributeValue(AMyFamily::my_number1);
            // n1 vaut 324
            $n2=$doc->getAttributeValue(AMyFamily::my_number2);
            // n2 vaut 765
            $doc->setAttributeValue(AMyFamily::my_sum, ($n1 + $n2));
            // la somme est de 1089
            $doc->store();
        }
    }

## Notes  {#core-ref:e5cba349-c3f7-41ba-ad8d-2214abb43ed5}

L'utilisation cette méthode dans une boucle qui potentiellement va instancier de
nombreux documents est déconseillé le temps de création d'un nouvel objet
document est important.

Pour une utilisation en boucle, il est préférable d'utiliser les classes
[`DocumentList`][doclist] ou [`SearchDoc`][searchdoc] qui manipule des listes de
documents.


## Voir aussi  {#core-ref:0dc39da2-2e73-471e-b8a4-157f3f51384b}

*   [`DocManager::getRawDocument()`][getrawdocument]
*   [`DocManager\cache::addDocument()`][addincache]

<!-- links -->
[getrawdocument]:   #core-ref:27f42abc-23c2-43c7-9a28-cfd32250632c
[searchdoc]:        #core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255
[doclist]:          #core-ref:23c71c28-dbce-4d34-819a-50d5bc4a38c3
[new_doc]:          #core-ref:e978cbd1-5f54-4a06-a6be-f1c079c2d734
[DocClass]:         #core-ref:1d557fb4-4eca-4ab8-a334-974fe563ddd2
[addincache]:       #core-ref:15d6a036-3b6e-4dbd-a0fe-361b925e6186