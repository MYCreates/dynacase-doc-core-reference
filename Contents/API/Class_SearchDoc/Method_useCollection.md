# SearchDoc::useCollection() {#core-ref:881c9fcb-81c2-45af-b89f-70be3a7f24b7}

<div class="short-description">
Cette méthode permet de définir que la recherche a pour base une collection, 
seul les documents contenu dans cette collection sont trouvés.
</div>


## Description {#core-ref:6726ff78-e93a-4b14-ac17-b8d2f0298f1d}

    [php]
    bool useCollection( int $dirid )

La collection indiquée en entrée dans la fonction peut être soit un dossier, 
une recherche ou encore un rapport.

### Avertissements {#core-ref:54e9705a-8bf0-4325-b595-e5f88226d9c6}

Aucun.

## Liste des paramètres {#core-ref:eea6abe1-2f54-49e0-bd17-c643a9244d11}

(string|int) `dirid`
:   La fonction prend en entrée la référence vers une collection que ça soit 
    par son nom logique ou par son identifiant.

## Valeur de retour {#core-ref:d4600e90-91c9-4bc1-a83c-c942473e9a43}

(bool) `true` si le paramètre a pu être appliqué `false` sinon.

## Erreurs / Exceptions {#core-ref:9e66ad5c-8c28-4f07-8497-d616640bc3f0}

Si jamais la référence passée en entrée n'est pas valide la fonction retourne
`false` et enregistre un message d'erreur sur l'objet 
(voir [`SearchDoc::getError`][searchDocGetError])

## Historique {#core-ref:b89f1e07-24b7-4664-b6bd-c95b28e41e73}

Aucun.

## Exemples {#core-ref:df3ddd1e-0aca-4de4-8786-e7e09942e28f}

Récupérer le contenu d'un dossier

    [php]
    $folderName="MY_FAVORITE_FOLDER"; // identifiant d'un document recherche ou dossier
    
    $searchDoc = new SearchDoc();
    $searchDoc->setObjectReturn(false);
    $searchDoc->useCollection($folderName);
    $rawResults= $searchDoc->search();
    if ($searchDoc->searchError()) {
        $action->exitError(sprintf("search error : %s",$searchDoc->getError()));
    }

Ceci est équivalent au code suivant :

    [php]
    $folder=\Dcp\DocManager::getDocument( "MY_FAVORITE_FOLDER");
    $rawResults=$folder->getContent();


Récupérer les sous-dossiers d'un dossier

    [php]
    $folderName="MY_FAVORITE_FOLDER"; // identifiant d'un dossier
    
    $searchDoc = new SearchDoc('', 'DIR');
    $searchDoc->setObjectReturn();
    $searchDoc->useCollection($folderName);
    $dl=$searchDoc->search()->getDocumentList();
    
    foreach ($dl as $doc) {
        printf("%d) %s\n", $doc->id, $doc->getTitle());
    }


## Notes {#core-ref:5bb0ac80-40d8-4eb2-8232-1ef47471c64f}

Aucune.

## Voir aussi {#core-ref:3dcb20cd-3e84-4d9b-ad56-956af84494df}

*   [`SearchDoc::setRecursiveSearch()`][setrecursivefolder]

Le chapitre avancé sur la [recherche dans les collections][advancedUseCollection].


<!-- links -->
[searchDocGetError]:        #core-ref:e57302ed-319e-4d63-b817-7a22d0ead3f2
[advancedUseCollection]:    #core-ref:7f084be4-ceb8-464d-81e4-8902da361aff
[setrecursivefolder]:       #core-ref:b99a6125-5a8b-420b-b1ce-f6a459f11612