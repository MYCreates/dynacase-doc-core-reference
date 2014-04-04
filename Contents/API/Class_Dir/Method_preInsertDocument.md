# Dir::preInsertDocument() {#core-ref:2f9580d7-cd06-4d09-8853-ed95f614d665}

<div class="short-description" markdown="1">

La méthode `preInsertDocument` est appelée par la méthode
[`insertDocument`][Dir::insertDocument] ou
[`insertMultipleDocuments`][Dir::insertMultipleDocuments] avant l'insertion du
document dans le Dossier (si l'argument
[`noprepost`][Dir::insertDocument_noprepost] est égal à `false`).

</div>

## Description {#core-ref:5c855b71-00f7-49a5-aa57-b08671745fb7}

    [php]
    string preInsertDocument ( string $docid,
                                 bool $multiple = false )



### Avertissements {#core-ref:dd8b8823-1f92-4e4e-9a52-5420e7dff7be}

Aucun.

## Liste des paramètres {#core-ref:87c362c7-2a37-4b90-a96d-b4c239bcbde8}

(string) `docid`
:   L'identifiant (identifiant numérique ou nom logique) du document qui va être
    inséré dans le Dossier.

(bool) `multiple`
:   `multiple` est positionné par la méthode
    [`insertMultipleDocuments`][Dir::insertMultipleDocuments] pour indiquer que
    `preInsertDoucment` est appelé dans le cadre de l'insertion de plusieurs
    documents.
    
    Valeurs possibles :
    
    * `true` : Indique que le document inséré l'est dans le cadre de l'insertion
      de plusieurs documents.
    * `false` : Indique qu'un seul document est inséré dans le dossier.

## Valeur de retour {#core-ref:213bcacd-784e-4e16-bfab-ba2f20dcf851}

La méthode retourne une chaîne vide s'il n'y a pas d'erreurs, ou une chaîne non-
vide contenant le message d'erreur dans le cas contraire.

## Erreurs / Exceptions {#core-ref:1fecd246-46ff-4267-b0c5-cb4a0ed3d4e9}

Si la méthode `preInsertDocument` retourne une chaîne non-vide, alors
l'insertion n'est pas faite, et la méthode
[`insertDocument`][Dir::insertDocument] retourne avec le message retourné par
`preInsertDocument`.

## Historique {#core-ref:51e8186d-cbb8-4320-93f9-8f8ea097be86}

### Release 3.2.5 {#core-ref:18d411cb-8f30-478f-87f5-0d52c145b491}

La méthode `preInsertDocument` remplace la méthode précédemment nommée
`preInsertDoc`.

L'utilisation de `preInsertDoc` est obsolète depuis la version 3.2.5 de
dynacase-core.

## Exemples {#core-ref:9face90e-fa4b-443f-8119-7188715424d6}

    [php]
    namespace Facturation;
    
    class ArchiveFacture extends \Dcp\Family\Dir {
        public function preInsertDocument($docId, $multiple = false) {
            $err=parent::preInsertDocument($docId, $multiple);
            if (empty($err)) {
                $facture = \Dcp\DocManager::getDocument( $docId); // prendre la dernière révision
                if ($facture) {
                    /*
                     * Seule les factures payés peuvent
                     * être insérées dans ce dossier.
                     */
                    if ($facture->isPaid()) {
                        return sprintf(
                            "La facture '%s' ne peut être archivée car elle n'est pas payée.",
                            $facture->getTitle()
                        );
                    }
                }
            }
            return $err;
        }
        protected function isPaid() {
            // Test de facturation
        }
    }

## Notes {#core-ref:b9e3945e-1945-4914-9c2c-45a4a63a9384}

Aucune.

## Voir aussi {#core-ref:de2d7c00-c12b-4a1b-912b-75e1ba3b4f44}

- [`Dir::postInsertDocument`][Dir::postInsertDocument]
- [`Dir::insertDocument`][Dir::insertDocument]
- [`Dir::insertMultipleDocuments`][Dir::insertMultipleDocuments]
- [`Dir::preInsertMultipleDocuments`][Dir::preInsertMultipleDocuments]
- [`Dir::removeDocument`][Dir::removeDocument]

<!-- links -->
[Dir::postInsertDocument]: #core-ref:65ec2b4a-8878-4004-8e42-0de8c359a231
[Dir::insertDocument]: #core-ref:9575ff95-480a-4dfb-9cd0-b89f44c3fad7
[Dir::insertMultipleDocuments]: #core-ref:098cf44e-568d-4dd2-8dd0-e2f104bc8615
[Dir::removeDocument]: #core-ref:d337e186-8066-49e2-92a0-26aa518cbf41
[Dir::insertDocument_noprepost]: #core-ref:f4d478ce-8145-4975-801e-d6158cb8b7d2
[Dir::preInsertMultipleDocuments]: #core-ref:f2b05730-8eca-43de-a9f4-6075c7f19d66
