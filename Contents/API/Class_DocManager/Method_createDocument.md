# Dcp\DocManager::createDocument()  {#core-ref:2f5afd12-1db3-4c69-a0fa-4b7fb044b723}

<div class="short-description">
Créé un objet document.
</div>

## Description  {#core-ref:e6362419-d485-4c4a-b618-c1c6f507789f}

    [php]
    static Dcp\Family\Document createDocument(
          int|string $familyIdentifier,
                bool $control=true,
                bool $useDefaultValues=true) 

Créer un objet [document][DocClass]. La classe de l'objet retourné est
fonction de la famille du document. L'objet document n'a pas encore
d'identifiant. Son identifiant est donné lors de l'enregistrement
([`Doc::store()`][docstore]).

### Avertissements  {#core-ref:fab5c4ae-5d16-4316-ba0e-bf620b9e4d34}

Le document n'est pas mis en cache.

## Liste des paramètres  {#core-ref:49ba77a1-e837-46a4-8c96-9b4bd41006a1}


(int|string) `familyIdentifier`
:   Identifiant de la famille du document à créer

(bool) `control`
    Indique s'il faut vérifier le droit de créer. 
    
(bool) `useDefaultValues`
    Indique s'il faut initialiser les attributs avec leurs valeurs par défaut.


## Valeur de retour  {#core-ref:c56ff484-79ce-4183-b4ce-1020ed320e8f}

Retour un objet de la classe de la famille indiquée.


## Erreurs / Exceptions  {#core-ref:0e8a72e8-e6fc-46a7-a4d3-7741f6cb0b90}

Exception `\Dcp\DocManager\Exception` :

*   `DMG0001`: Si l'identifiant de famille n'est pas syntaxiquement valide.
    L'identifiant doit être un nombre positif ou une  chaine de caractère non
    vide.
*   `DMG0002`: Si la famille n'existe pas.
*   `DMG0003`: Si le droit de créer n'est pas accordé (droit `create`). 



## Historique  {#core-ref:137e4413-68ab-4120-a2ba-c8297e01c421}

Remplace la fonction [`createDoc`][createdoc].

## Exemples  {#core-ref:2eb81df0-63bc-46bd-978f-c463f0279092}

### Création simple {#core-ref:1163629f-0ab2-4abf-b542-da0488b96384}

    [php]
    use AMyFamily as \Dcp\AttributeIdentifiers\MyFamily
    use MyFamily as \Dcp\Family\MyFamily
    
    $doc=\Dcp\DocManager::createDocument(MyFamily::familyName);
    $doc->setAttributeValue(AMyFamily::my_number1, 345);
    $doc->setAttributeValue(AMyFamily::my_number2, 654);
    $err=$doc->store();
    if (empty($err)) {
        printf("Nouveau document %s n°%d", $doc->getTitle(), $doc->id);
    }

### Avec traitement des exceptions {#core-ref:226a6772-6641-4d9c-b22e-d676fe922ff7}

    [php]
    function createWithValues($family, array $values) {
        try {
            $doc=\Dcp\DocManager::createDocument($family);
            foreach (values as $attrid=>$value) {
                $doc->setAttributeValue($attrid, $value);
            }
            $err=$doc->store();
            if (empty($err)) {
                printf("Nouveau document %s n°%d\n", $doc->getTitle(), $doc->id);
            }
        } catch (Dcp\DocManager\Exception $e) {
            switch ($e->getDcpCode()) {
                case"DMG0002":
                    // famille inconnue
                    printf("Création impossible [%s]\n", $family);
                    break;
                default:
                    // autre erreur
                    printf("Création impossible [%s]\n", $e->getMessage());
                
            }
        } catch (Dcp\Exception $e) { 
            switch ($e->getDcpCode()) {
                case "DOC0115":
                    // attribut inconnu
                    printf("Erreur dans l'affectation [%s]\n", $e->getDcpMessage());
                    break;
                default:
                    // autre erreur
                    printf("Affectation impossible [%s]\n", $e->getMessage());
            }
        }
    }

## Notes  {#core-ref:c2a4b0a5-9b0f-4a3d-abb3-f277023030ce}

L'identifiant de la famille utilisée en paramètre n'est pas sensible à la casse.

## Voir aussi  {#core-ref:6c2ef8a0-4628-42a3-8c67-c00c7f3a6217}

*   [\Dcp\DocManager\createTemporaryDocument()][createtemporarydoc]

<!-- links -->
[getrawdocument]:   #core-ref:27f42abc-23c2-43c7-9a28-cfd32250632c
[searchdoc]:        #core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255
[doclist]:          #core-ref:23c71c28-dbce-4d34-819a-50d5bc4a38c3
[DocClass]:         #core-ref:1d557fb4-4eca-4ab8-a334-974fe563ddd2
[docstore]:         #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[createdoc]:        #core-ref:9886581a-243a-4c78-8490-8fda2209fd93
[createtemporarydoc]:        #core-ref:5e5cda73-398a-49fd-a14f-24ee877cf3fa