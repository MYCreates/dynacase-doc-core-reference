# Dir::postInsertDocument() {#core-ref:65ec2b4a-8878-4004-8e42-0de8c359a231}

<div class="short-description" markdown="1">

La méthode `postInsertDocument` est appelée par la méthode
[`insertDocument`][Dir::insertDocument] ou
[`insertMultipleDocuments`][Dir::insertMultipleDocuments] après l'insertion du
document dans le Dossier (si [`noprepost`][Dir::insertDocument_noprepost] est
égal à `false`).

</div>

## Description {#core-ref:431e47f9-63f0-4818-b5d3-674d0b082893}

    [php]
    string postInsertDocument ( string $docid,
                                  bool $multiple = false )

### Avertissements {#core-ref:11fe248c-b22b-47ad-b541-b5849e4e41ae}

Aucun.

## Liste des paramètres {#core-ref:6462a085-5bda-439f-92a2-efec348f8daa}

(string) `docid`
:   L'identifiant (identifiant numérique ou nom logique) du document qui a été
    inséré dans le Dossier.

(bool) `multiple`
:   `multiple` est positionné par la méthode
    [`insertMultipleDocuments`][Dir::insertMultipleDocuments] pour indiquer que
    `postInsertDoucment` est appelé dans le cadre de l'insertion de plusieurs
    document.
    
    Valeurs possibles :
    
    * `true` : Indique que le document inséré l'est dans le cadre de l'insertion
      de plusieurs documents.
    * `false` : Indique qu'un seul document est inséré dans le dossier.

## Valeur de retour {#core-ref:bf232ec7-1704-4b4d-b240-d369a8afe6d1}

La méthode retourne une chaîne vide s'il n'y a pas d'erreurs, ou une chaîne non-
vide contenant le message d'erreur dans le cas contraire.

## Erreurs / Exceptions {#core-ref:5ddee4aa-4fbb-4991-8dca-e64c59b04b8c}

Aucune.

## Historique {#core-ref:0a76ba2c-aabd-4e81-96dc-67395342d6c3}

### Release 3.2.5 {#core-ref:55c45e02-8598-4953-abe9-531498ccf2df}

La méthode `postInsertDocument` remplace la méthode précédemment nommée
`postInsertDoc`.

L'utilisation de `postInsertDoc` est obsolète depuis la version 3.2.5 de
dynacase-core.

## Exemples {#core-ref:50a05a82-c8c7-42f4-9241-8d50f2107295}

    [php]
    namespace Facturation;
    
    class ArchiveFacture extends \Dcp\Family\Dir {
    
        public function postInsertDocument($docId, $multiple = false) {
            $err=parent::postInsertDocument($docId, $multiple);
            if (empty($err)) {
                /*
                 * Appeler la méthode archiverFacture() des
                 * factures insérées dans le dossier.
                 */
                $facture = \Dcp\DocManager::getDocument( $docId, true);// prendre la dernière révision
                $facture->archiverFacture();
            }
            return $err;
        }
    
        protected function archiverFacture() {
            // Archivage facturation
        }
    }

## Notes {#core-ref:f717e158-598e-46ab-94e1-d7225b8e2e04}

Aucune.

## Voir aussi {#core-ref:1d3be4fc-7939-4929-b068-871077a2e097}

- [`Dir::preInsertDocument`][Dir::preInsertDocument]
- [`Dir::insertDocument`][Dir::insertDocument]
- [`Dir::insertMultipleDocuments`][Dir::insertMultipleDocuments]
- [`Dir::removeDocument`][Dir::removeDocument]

<!-- links -->
[Dir::preInsertDocument]: #core-ref:2f9580d7-cd06-4d09-8853-ed95f614d665
[Dir::insertDocument]: #core-ref:9575ff95-480a-4dfb-9cd0-b89f44c3fad7
[Dir::insertMultipleDocuments]: #core-ref:098cf44e-568d-4dd2-8dd0-e2f104bc8615
[Dir::removeDocument]: #core-ref:d337e186-8066-49e2-92a0-26aa518cbf41
[Dir::insertDocument_noprepost]: #core-ref:f4d478ce-8145-4975-801e-d6158cb8b7d2
