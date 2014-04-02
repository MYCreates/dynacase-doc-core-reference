# Dcp\DocManager::getDocumentFromRawDocument()  {#core-ref:aa0098ed-9007-4bff-a9a6-4d92f0ed1218}

<div class="short-description">
Retourne un objet document en fonction des données brutes.
</div>


## Description  {#core-ref:438db2b0-bcb3-45b6-a9ae-3764c59a243b}

    [php]
    static Dcp\Document Dcp\DocManager::getDocumentFromRawDocument ( 
                                               string[] $rawDocument) 

Retourne un objet [_Document_][DocClass] en fonction des données brutes. La
classe de l'objet retourné est fonction de la famille du document.

### Avertissements  {#core-ref:6b3db2f4-cd82-4933-a2e6-063791a250a2}

Le cache n'est pas utilisé.  
Même si le document indiqué en paramètre est présent dans le cache, le document 
retourné sera tel que celui précisé en argument.

## Liste des paramètres  {#core-ref:3ac56eca-694b-4ede-830a-d97f3dbfa856}

(string[]) `rawDocument`
:   Données brutes d'un document.



## Valeur de retour  {#core-ref:6087256b-cc72-412d-8de1-716416e8dd21}

Retourne un objet document en fonction des données brutes. La classe de l'objet
retourné est fonction de la famille du document.

Retourne `null` si l'identifiant ne correspond à aucun document.

## Erreurs / Exceptions  {#core-ref:af176c69-b195-485b-9b3b-30ee5b991ef2}

Exception `\Dcp\DocManager\Exception` :

*   si l'identifiant n'est pas
    syntaxiquement valide. L'identifiant doit être un nombre positif ou une chaîne
    de caractère non vide.
*   Si le tableau de donnée ne correspond pas à un document. Il doit contenir
    l'ensemble des propriétés.


## Historique  {#core-ref:d41d1330-136c-4d90-a03c-486ccc1cf9be}

Aucun.

## Exemples  {#core-ref:763002ea-76f0-4b1b-9fdd-f4aae37084d6}

Conversion depuis `Dcp\DocManager::getRawDocument`.
La document `1234` est de la famille `MY_FAMILY`.

    [php]
    use AMyFamily as \Dcp\AttributeIdentifiers\MyFamily
    $rawDoc=\Dcp\DocManager::getRawDocument(1234);
    if ($rawDoc !== null) {
        // le titre
        print $rawDoc["title"];
        // l'attribut my_number1
        print $rawDoc[AMyFamily::my_number1];
        
        $doc=\Dcp\DocManager::getDocumentFromRawDocument($rawDoc);
        if ($doc !== null) {
            print $doc->getTitle();
        }
    }

## Notes  {#core-ref:bdeae98c-d6e2-4a07-ada7-ed82f4664dd6}

Aucun appel en base de données n'est fait pour récupérer les données du document.

## Voir aussi  {#core-ref:2eb5c727-c60b-48b4-8490-519feff1a860}

*   [`DocManager::getDocument()`][getdocument]
*   [`DocManager::getRawDocument()`][getrawdocument]


<!-- links -->
[getrawdocument]:   #core-ref:27f42abc-23c2-43c7-9a28-cfd32250632c
[getdocument]:      #core-ref:dfa0762f-6ff3-4349-bd21-6442740d9dcc
[DocClass]:         #core-ref:1d557fb4-4eca-4ab8-a334-974fe563ddd2