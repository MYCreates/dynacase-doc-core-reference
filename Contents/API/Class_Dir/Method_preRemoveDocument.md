# Dir::preRemoveDocument() {#core-ref:251b17bf-af38-4fdc-a9cc-01074279ddb5}

<div class="short-description" markdown="1">

La méthode `preRemoveDocument` est appelée par la méthode
[`removeDocument`][Dir::removeDocument] avant la suppression d'un document dans
le dossier (si [`noprepost`][Dir::removeDocument_noprepost] est égal à `false`).

</div>

## Description {#core-ref:e0b9c96f-e7c1-4bce-8d60-82c9607dc3ca}

    [php]
    string preRemoveDocument ( string $docid )

### Avertissements {#core-ref:e93f1b8b-1ec6-47eb-8ff8-6fcfc4197d7a}

Aucun.

## Liste des paramètres {#core-ref:cf5df222-8edb-44e9-a676-f0e493087158}

(string) `docid`
:   L'identifiant (identifiant numérique ou nom logique) du document à enlever
    du dossier.

## Valeur de retour {#core-ref:e3dc9cc2-3c7a-4701-98bb-45ba2a2b08e5}

La méthode retourne une chaîne vide s'il n'y a pas d'erreurs, ou une chaîne non-
vide contenant le message d'erreur dans le cas contraire.

## Erreurs / Exceptions {#core-ref:459b22dc-c90f-4e72-9aa5-a56047f1a529}

Si la méthode `preRemoveDocument` retourne une erreur, alors la suppression du
document dans le dossier n'est pas effectuée.

## Historique {#core-ref:84b8ec7f-e9d7-4c70-9429-849a199bb266}

### Release 3.2.5 {#core-ref:bee67ab5-b785-4793-932c-51a9b71f70e9}

La méthode `preRemoveDocument` remplace la méthode précédemment nommée
`preUnlinkDoc`.

L'utilisation de `preUnlinkDoc` est obsolète depuis la version 3.2.5 de
dynacase-core.

## Exemples {#core-ref:6378f42a-84e0-417e-a687-a38717ea1f79}

    [php]
    
    namespace Facturation;
    
    class ArchiveFacture extends \Dcp\Family\Dir {
    
        public function preRemoveDocument($docId) {
            $facture = \Dcp\DocManager::getDocument( $docId); // prendre la dernière révision
            if ($facture && !$facture->isPaid()) {
            	return sprintf(
            		'La facture doit être payée pour pouvoir être enlevée du dossier.'
                );
            }
            return '';
        }
    
    }    

## Notes {#core-ref:0e1d8b12-7f34-4ca2-b2c5-04a852cfcea7}

Aucune.

## Voir aussi {#core-ref:66338ab9-bd44-4669-ad49-0f6fd635ffca}

- [`Dir::removeDocument`][Dir::removeDocument]
- [`Dir::postRemoveDocument`][Dir::postRemoveDocument]

<!-- links -->
[Dir::removeDocument]: #core-ref:d337e186-8066-49e2-92a0-26aa518cbf41
[Dir::postRemoveDocument]: #core-ref:efd55fb9-600e-42fa-a7c5-0b3b31ea3cc3
[Dir::removeDocument_noprepost]: #core-ref:02b27db3-0b22-4f21-a78d-c401fe357c2b
