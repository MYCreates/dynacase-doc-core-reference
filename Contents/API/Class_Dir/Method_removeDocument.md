# Dir::removeDocument() {#core-ref:d337e186-8066-49e2-92a0-26aa518cbf41}

<div class="short-description" markdown="1">

La méthode `removeDocument` permet d'enlever un document du dossier.

</div>

## Description {#core-ref:b1ba752b-e530-429e-aa66-6707beeed7e7}

    [php]
    string removeDocument ( string $docid,
                              bool $noprepost = false,
                              bool $nocontrol = false )

Cette méthode enlève une reférence de document au dossier.

### Avertissements {#core-ref:80577b56-a84c-4c00-8409-e57b9a9c6a89}

Le document n'est pas supprimé. Seule sa référence au dossier est supprimée.

## Liste des paramètres {#core-ref:02b27db3-0b22-4f21-a78d-c401fe357c2b}

(string) `docid`
:   L'identifiant (identifiant numérique ou nom logique) du document à enlever
    du dossier.

(bool) `noprepost`
:   `noprepost` permet de désactiver l'appel des méthodes hameçons
    [`preRemoveDocument`][Dir::preRemoveDocument] et
    [`postRemoveDocument`][Dir::postRemoveDocument] appelées respectivement
    avant et après la suppression du document dans le dossier.
    
    Valeur par défaut : `false`.
    
    Si `false`, les hameçons [`preRemoveDocument`][Dir::preRemoveDocument] et
    [`postRemoveDocument`][Dir::postRemoveDocument] sont appelés.  
    Si `true`, les hameçons ne sont pas appelés.  

(bool) `nocontrol`
:   `nocontrol` permet de désactiver le contrôle du 
  [droit de modification][pdir] du dossier .
    
    Valeur par défaut : `false`.
    
    Si `false`, le contrôle du droit de modification du dossier est effectué.  
    Si `true`, le contrôle de droit n'est pas effectué.

## Valeur de retour {#core-ref:91631212-4b6d-433c-a702-c46b98446d29}

La méthode retourne une chaîne vide s'il n'y a pas d'erreurs, ou une chaîne non-
vide contenant le message d'erreur dans le cas contraire.

## Erreurs / Exceptions {#core-ref:59e9e4fe-71b9-444a-b2b9-edef13fc7464}

Les erreurs peuvent êtres :

* L'utilisateur ne possède pas le droit de modifier le contenu du dossier (voir
  paramètre `nocontrol` ci-dessus).
* Les méthodes d'hameçon [`preRemoveDocument`][Dir::preRemoveDocument] ou
  [`postRemoveDocument`][Dir::postRemoveDocument] ont retourné une erreur (voir
  paramètre `noprepost` ci-dessus).

Si le document à enlever n'est pas présent, aucune erreur n'est retournée.

## Historique {#core-ref:a8e9a225-d3ac-413e-9325-bcf357a16262}

### Release 3.2.5 {#core-ref:721ccaa9-e7e0-47d8-a166-b881b5597424}

La méthode `removeDocument` remplace la méthode précédemment nommée `delFile`.

L'utilisation de `delFile` est obsolète depuis la version 3.2.5 de dynacase-
core.

## Exemples {#core-ref:7b7badcd-3fef-4834-97fd-4461684289ce}

    [php]
    $dossier = \Dcp\DocManager::getDocument( 'MY_FOLDER');
    
    /*
     * Rechercher tous les documents référencés dans le dossier
     */
    $s = new SearcDoc('');
    $s->useCollection($dossier->id);
    $docList = $s->search();
    
    /*
     * Enlever les documents du dossier
     */
    foreach ($docList as $doc) {
    	$err = $dossier->removeDocument($doc['initid']);
    	if ($err != '') {
    		printf("Error removing document '%d' from DIR: %s", $doc['id'], $err);
    	}
    }

## Notes {#core-ref:7ad23513-abdb-4552-a87e-4e63a0805292}

Ordre d'appel des hameçons :

    .-(1)-------------------------------.
    | $this->preRemoveDocument($docId); |
    '-----------------------------------'    
    
    <Suppression du document $docId dans le dossier>
    
    .-(2)--------------------------------.
    | $this->postRemoveDocument($docId); |
    '------------------------------------'

## Voir aussi {#core-ref:6360705f-7989-4729-a5b4-877e57d4454c}

- [`Dir::preRemoveDocument`][Dir::preRemoveDocument]
- [`Dir::postRemoveDocument`][Dir::postRemoveDocument]

<!-- links -->
[Dir::preRemoveDocument]:   #core-ref:251b17bf-af38-4fdc-a9cc-01074279ddb5
[Dir::postRemoveDocument]:  #core-ref:efd55fb9-600e-42fa-a7c5-0b3b31ea3cc3
[pdir]:                     #core-ref:0cd3fe9a-57cf-481f-8fc0-560bc71d6430