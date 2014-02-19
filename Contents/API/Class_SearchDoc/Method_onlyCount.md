# SearchDoc::onlyCount() {#core-ref:2d43be1a-1991-42dd-a25d-5c3bb0b393fa}

<div class="short-description">
Cette méthode permet de ne faire que le décompte des documents.
</div>


## Description {#core-ref:6016b83d-e4fc-4e6e-87dd-ab7eeee3dc7a}

    [php]
    int onlyCount ()

Cette méthode permet d'avoir uniquement le nombre de documents trouvés par la 
requête sans pour autant récupérer ces documents de la base de données.

### Avertissements {#core-ref:a42d9ab4-4e62-4519-aed3-cd93624cbc49}

Un nouvel appel à la base de données est effectué à chaque appel et écrase le
précédent résultat fait par [`search()`][search] ou le précédent `onlyCount`.

## Liste des paramètres {#core-ref:14ff45a3-1478-4e9d-a024-3aee916fb23a}

Aucun.

## Valeur de retour {#core-ref:98890fb4-34d7-4743-9035-eb88bc026f1c}

`int`
:   Le nombre de documents trouvés par le searchDoc.

## Erreurs / Exceptions {#core-ref:14835f07-019e-4fc0-ae07-33510d648027}

Exception `\Dcp\Db\Exception` en cas d'erreur de requête sql.

## Historique {#core-ref:b2d103c2-4aba-46b6-bfd7-1e933aab9a87}

### Release 3.2.12 {#core-ref:b9a63af6-43a8-484b-942d-99963e29a8fc}

La méthode `onlyCount()` récupère toujours le résultat en envoyant une requête
en base de donnée. Auparavant, si la méthode [`search()`][search] ou `onlyCount`
étaient préalablement exécutée la méthode ne renvoyait pas de requête mais
récupéré le dernier compte effectué. Il fallait dans ce cas appeler la méthode
[`reset()`][reset] pour forcer un recomptage.

## Exemples {#core-ref:d5c199d8-e3d2-4a9a-8b5a-0b76f0741155}

Exemple de searchDoc :

    [php]
    function testOnlyCount(Action & $action)
    {
        header('Content-Type: text/plain');
        
        $searchDoc = new searchDoc();
        var_export($searchDoc->onlyCount());
        print "\n";
        var_export($searchDoc->getSearchInfo());
        print "\n";
        print "\n";
        $searchDoc->addFilter("title = 'toto'");
        var_export($searchDoc->onlyCount());
        print "\n";
        var_export($searchDoc->getSearchInfo());
        print "\n";
        print "\n";
        $searchDoc = new searchDoc();
        $searchDoc->search();
        var_export($searchDoc->onlyCount());
        print "\n";
        var_export($searchDoc->getSearchInfo());
        print "\n";
        print "\n";
        $searchDoc->addFilter("title = 'toto'");
        var_export($searchDoc->onlyCount());
        print "\n";
        var_export($searchDoc->getSearchInfo());
        print "\n";
        print "\n";
        
    }

    1493
    array (
<!--beware there is no tab here for sql syntax-->

    [sql]
      'query' => 'select count(docread.id) from  docread  where   (docread.archiveid is null) and (docread.doctype != \'Z\') and (docread.doctype != \'T\') and (docread.locked != -1)',

      'delay' => '0.003s',
    )
    
    0
    array (
<!--beware there is no tab here for sql syntax-->

    [sql]
      'query' => 'select count(docread.id) from  docread  where   (docread.archiveid is null) and (docread.doctype != \'Z\') and (docread.doctype != \'T\') and (docread.locked != -1) and (title = \'toto\')',

      'delay' => '0.001s',
    )
    
    1493
    array (
      'count' => 1493,
<!--beware there is no tab here for sql syntax-->

    [sql]
      'query' => 'select docread.*  from  docread  where   (docread.archiveid is null) and (docread.doctype != \'Z\') and (docread.doctype != \'T\') and (docread.locked != -1) ORDER BY title LIMIT ALL OFFSET 0;',

      'error' => '',
      'delay' => '0.073s',
    )
    
    1493
    array (
      'count' => 1493,
<!--beware there is no tab here for sql syntax-->

    [sql]
      'query' => 'select docread.*  from  docread  where   (docread.archiveid is null) and (docread.doctype != \'Z\') and (docread.doctype != \'T\') and (docread.locked != -1) ORDER BY title LIMIT ALL OFFSET 0;',

      'error' => '',
      'delay' => '0.073s',
    )

## Notes {#core-ref:ee038431-ef60-41c1-b52f-f958a3923b44}

Aucune.

## Voir aussi {#core-ref:a2d49a00-ca37-4120-8ada-f0a80a3791bd}

*   [`SearchDoc::count()`][count]

<!-- links -->

[search]:       #core-ref:6f5cc024-66e4-429e-9071-67d4523a8e08
[reset]:        #core-ref:18f98a7d-7db0-4270-97b2-0a1759a5b0e6
[count]:        #core-ref:8daca9d1-69e9-4871-b661-d710b8727d41