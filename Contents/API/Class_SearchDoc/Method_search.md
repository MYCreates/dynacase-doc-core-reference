# SearchDoc::search() {#core-ref:6f5cc024-66e4-429e-9071-67d4523a8e08}

<div class="short-description">
Cette méthode permet d'exécuter la recherche.
</div>

## Description {#core-ref:aeb7ad6b-d951-4a5b-b17a-52cc4db4e51e}

    [php]
    array|null|SearchDoc search (  )

Cette méthode exécute la recherche. Elle construit la requête `SQL` nécessaire
en utilisant les paramètres de la famille recherchée et les conditions insérées
par les différentes méthodes.

### Avertissements {#core-ref:f9475914-b820-462f-a0b1-47c0a98d8836}

Par défaut certains types de documents sont exclus des [résultats de la
recherche][advancedExcluded]. Ces exclusions sont modifiables par les méthodes
[`::excludeConfidential()`][excludeConfidential],
[`::overrideViewControl()`][overrideViewControl] et les  attributs
[`trash`][searchprop] et [`distinct`][searchprop].

## Liste des paramètres {#core-ref:1182a98e-413d-4fd5-8cf7-bf285852e5a3}

Aucun.

## Valeur de retour {#core-ref:9d82c815-d0af-49d7-983d-4a9bf85450a0}

La valeur du retour [dépend du type de recherche][advancedSearch] :

-   Résultats bruts : le retour est alors un tableau `array`, si la préparation de
    la recherche à échoué alors le résultat est un tableau vide,
-   Résultats documents : le retour est alors l'objet `searchDoc` lui-même ou
    `null` si la préparation de la recherche a échoué.

**Note** : Le changement du type de recherche se fait uniquement avant que la 
recherche ne soit déclenchée avec la méthode 
[`SearchDoc::setObjectReturn()`][setObjectReturn].

## Erreurs / Exceptions {#core-ref:15419ed0-38bb-4890-9732-0779e83882dd}

Si la préparation de la recherche a échoué alors si la recherche :

-   retourne des résultats brut : le retour est un array vide,
-   retourne des documents : le retour est `null`.

Exceptions :

-   `\Dcp\Db\Exception` si la requête n'a pas pu être exécutée.
-   `\Dcp\SearchDoc\Exception("SD0008")` si la méthode [`SearchDoc::addFilter`][core-ref:SearchDoc::addFilter]
    est utilisée conjointement avec une [recherche spécialisée][core-ref:ssearch] en tant que
    [collection de base][core-ref:SearchDoc::useCollection].

## Historique {#core-ref:c718b9c4-c3aa-4666-ac81-9af3286235cb}

Aucun.

## Exemples {#core-ref:382458c1-64ec-4b59-ad90-33ac39265854}

### Recherche simple  {#core-ref:db2507b5-cfc9-4d2b-87da-a29914223194}

Recherche de tous les dossiers que l'utilisateur courant peut voir.

    [php]
    $searchDoc=new SearchDoc("","DIR");
    $searchDoc->setObjectReturn(true);
    $documentList=$searchDoc->search()->getDocumentList();
    foreach ($documentList as $docid=>$doc) {
      printf("%d) %s (%s)", 
           $docid,
           $doc->getTitle(),
           $doc->getRawValue(\Dcp\AttributeIdentifiers\Dir::ba_title));
    }

### Recherche avec capture d'erreurs {#core-ref:f860cc7b-8fe4-4f9d-a151-35c65bf3bb4a}

Reprise de l'exemple précédent en ajoutant les tests d'erreurs.

    [php]
    try {
        $searchDoc=new SearchDoc("","DIR");
        $searchDoc->setObjectReturn(true);
        $searchDoc->search();
        
        if ($err=$searchDoc->searchError()) {
          throw new \Dcp\SearchDoc\Exception($err);
        }
        $documentList=$searchDoc->getDocumentList();
        
        foreach ($documentList as $docid=>$doc) {
          printf("%d) %s (%s)\n", 
                 $docid,
                 $doc->getTitle(),
                 $doc->getRawValue(\Dcp\AttributeIdentifiers\Dir::ba_title));
        }
    } catch (\Dcp\Db\Exception $e) {
        printf("Database Error: [%s]\n", $e->getMessage());
    } catch (\Dcp\SearchDoc\Exception $e) {
        printf("Search Error: [%s]\n", $e->getMessage());
    }

## Notes {#core-ref:c3321a4a-316b-4ccf-a0f9-9b866863f223}

Aucune.

## Voir aussi {#core-ref:ad16b97f-7fc8-4240-97f0-a36c13084446}

*   Chapitre [Mécanismes de recherche][recherche],
*   [Autres exemples][advancedSearch],
*   [`SearchDoc::setObjectReturn()`][setObjectReturn].


<!-- links -->

[advancedExcluded]:                 #core-ref:d0a89548-a743-4dfc-bf43-49192ef1b6a8
[advancedSearch]:                   #core-ref:7291dea8-a2db-46be-8194-bc6f100cc467
[core-ref:SearchDoc::addFilter]:    #core-ref:ec525c92-ab30-4861-aba1-7c2678df130a
[core-ref:ssearch]:                 #core-ref:1eb7c0ad-dabd-47e9-83bf-f9e3d4eca806
[excludeConfidential]:              #core-ref:17be152c-0844-40d5-bfc5-a9aa2f2695fc
[overrideViewControl]:              #core-ref:18f98a7d-7db0-4270-97b2-0a1759a5b0e6
[recherche]:                        #core-ref:bda916b0-e564-40fd-b195-c62bbab7b8be
[reset]:                            #core-ref:18f98a7d-7db0-4270-97b2-0a1759a5b0e6
[searchprop]:                       #core-ref:72ce807f-a392-4579-ab46-d58a3d18c214
[setObjectReturn]:                  #core-ref:3a0b4882-81ff-4030-9f60-a0ed0ff1f958
