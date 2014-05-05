# Doc::getCustomTitle() {#core-ref:3c5ff78d-c080-48fb-a293-9736ed4e95b8}

<div class="short-description" markdown="1">

La méthode `getCustomTitle` permet de définir le titre du document.

</div>

## Description {#core-ref:7db6c3f2-a6dc-48e7-8b9b-4e436a5f1203}

    [php]
    string getCustomTitle ( void )

La méthode `getCustomTitle` permet de surcharger la méthode de composition du
titre par défaut des documents, et de définir sa propre méthode de composition
du titre.

Par défaut, le titre est composé en concaténant la valeur des attributs
déclarés avec la caractéristique [`in_title`][attr_in_title].

### Avertissements {#core-ref:d7c909a8-f2fa-4ddf-954c-00704e9a694d}

Aucun.

## Liste des paramètres {#core-ref:a3b64962-eab4-4990-a769-ba7914b90fa2}

Aucun.

## Valeur de retour {#core-ref:b9a98633-25e6-41c8-8336-bd6a412c59fd}

La méthode retourne une chaîne de caractères qui est le titre du document.

## Erreurs / Exceptions {#core-ref:337d123b-e146-46be-81a5-e31f266a7f13}

Aucune.

## Historique {#core-ref:579d6848-b2d2-479d-a6f4-97ef2b904837}

### Release 3.2.5 {#core-ref:ad73426f-f010-4f29-bfee-a4939824039c}

La méthode `getCustomTitle` remplace la méthode précédemment nommée
`getSpecTitle`.

L'utilisation de `getSpecTitle` est obsolète depuis la version 3.2.5 de
dynacase-core.

## Exemples {#core-ref:e5ff480e-ce03-40a7-b098-116e03fd6ad0}

-   Exemple #1
    
        [php]
        namespace Facturation;
        
        class ArchiveFacture extends \Dcp\Family\Dir {
        
            /*
             * Compose le titre avec la valeur de l'attribut `ref_client` et
             * l'`id` du document. Ex. : "ACMECorp-1234"
             */
            public function getCustomTitle() {
                return sprintf(
                    "%s-%s",
                    $this->getAttributeValue(\Dcp\AttributeIdentifiers\ArchiveFacture::ref_client),
                    $this->getPropertyValue('id'),
                );
            }
        }

## Notes {#core-ref:49db0916-cd96-4cc8-972b-706f3d6a4b0d}

Lors de la construction d'un [rapport][rapport], le document est incomplet et
les valeurs d'attributs sont partiellement présents en fonction des colonnes
affichées. Si le titre est dépendant d'un attribut qui n'est pas présent, le
titre présenté dans le rapport peut être incomplet.

Pour s'assurer que le titre présenté est correct, il est possible de retourner
le titre "courant" lorsque le document est incomplet comme le montre l'exemple
suivant.

    [php]
    public function getCustomTitle() {
       if ($this->doctype === 'I') { // incomplete document
          return $this->title;
       } else {
          // Doing special title
          $mySpecialTitle = sprintf("REF/%d/%d/%s", 
                                 $this->initid, 
                                 $this->revision,
                                 $this->getAttributeValue(\Dcp\AttributeIdentifiers\ArchiveFacture::ref_client));
          return $mySpecialTitle;
       }
    } 

## Voir aussi {#core-ref:e5fb287c-b8e0-433c-b75a-85b15afc7a40}

- [Caractéristique `in_title` des attributs][attr_in_title]
- [Doc::getTitle][Doc::getTitle]

<!-- links -->
[attr_in_title]: #core-ref:b0e414c0-b795-4bbe-b70e-a308b7f1b4ab
[Doc::getTitle]: #core-ref:84011cc8-2aec-4f39-81f0-c7ae803e4913
[rapport]:       #core-ref:4d6429f1-b3ae-46c4-8687-1c8d29f92f37
