# Dir::postRemoveDocument() {#core-ref:efd55fb9-600e-42fa-a7c5-0b3b31ea3cc3}

<div class="short-description" markdown="1">

La méthode `postRemoveDocument` est appelée par la méthode
[`removeDocument`][Dir::removeDocument] après que le document ait été enlevé du
dossier (si [`noprepost`][Dir::removeDocument_noprepost] est égal à `false`).

</div>

## Description {#core-ref:05ee84b9-5703-49da-8bc9-97861a930891}

    [php]
    string postRemoveDocument ( string $docid )

### Avertissements {#core-ref:04dc09e3-1886-4e2e-bdee-4883e16ac945}

Aucun.

## Liste des paramètres {#core-ref:e6f6ee92-4cf8-4f77-866c-b164411758a6}

(string) `docid`
:   L'identifiant (identifiant numérique ou nom logique) du document qui a été
    supprimé dans le dossier.

## Valeur de retour {#core-ref:d7a867a3-15b2-4abf-a327-1c06abe65398}

La méthode retourne une chaîne vide s'il n'y a pas d'erreurs, ou une chaîne non-
vide contenant le message d'erreur dans le cas contraire.

## Erreurs / Exceptions {#core-ref:354eb066-af22-4d78-a4fa-88ac8300595a}

Aucune.

## Historique {#core-ref:519e0b75-616e-4aca-ab51-2780909a7380}

### Release 3.2.5 {#core-ref:7eb75d18-2b70-4d43-9199-f72d2cbcc9fe}

La méthode `postRemoveDocument` remplace la méthode précédemment nommée
`postUnlinkDoc`.

L'utilisation de `postUnlinkDoc` est obsolète depuis la version 3.2.5 de
dynacase-core.

## Exemples {#core-ref:45153702-b640-4deb-9a39-0d4bebf99367}

    [php]
    
    namespace Facturation;
    
    class ArchiveFacture extends \Dcp\Family\Dir {
    
        public function postRemoveDocument($docId) {
        	/*
        	 * Mettre la facture dans l'état 'Brouillon'
        	 * lorsqu'elle est enlevé du dossier
        	 */
        	$facture = \Dcp\DocManager::getDocument( $docId, true);
        	$facture->setState('ST_Brouillon');
        	return '';
        }
    
    }

## Notes {#core-ref:603cb9eb-8cba-4f8a-b547-65e746a35b12}

Aucune.

## Voir aussi {#core-ref:e6a9981a-c02e-48d0-974e-9ca354c07605}

- [`Dir::removeDocument`][Dir::removeDocument]
- [`Dir::preRemoveDocument`][Dir::preRemoveDocument]

<!-- links -->
[Dir::removeDocument]: #core-ref:d337e186-8066-49e2-92a0-26aa518cbf41
[Dir::preRemoveDocument]: #core-ref:251b17bf-af38-4fdc-a9cc-01074279ddb5
[Dir::removeDocument_noprepost]: #core-ref:02b27db3-0b22-4f21-a78d-c401fe357c2b
