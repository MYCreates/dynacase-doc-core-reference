# Transactions et savePoint  {#core-ref:32e0a8cb-0e8b-4f77-a62d-a45da16d39a8}

Des [transactions][transactionSQL] et des points de sauvegarde de base de donnés
peuvent être utilisés afin de [confirmer][commitSQL] ou de
[revenir][RollbackSQL] à un point de sauvegarde. Ces transactions n'ont aucun
effet sur l'objet mémoire manipulé.

Les méthodes `DbObj::savePoint()`, `DbObj::rollbackPoint()` et
`DbObj::commitPoint()` permettent de gérer les transactions et les points de
retours.

    [php]
    include_once("FDL/Class.Doc.php");
    $document = \Dcp\DocManager::getDocument(8160); // Doc dérive de DbObj
    $document->savePoint("One");
    $document->setValue("us_lname","Test one");
    $document->store();  // modification en base doc n°8160 à comme nom "Test One"
    
    $document->savePoint("Two");
    $document->setValue("us_lname","Test two");
    $document->store();// modification en base doc n°8160 à comme nom "Test Two"
    $document->rollbackPoint("Two"); // annulation de toutes les requêtes depuis le point "Two"
    
    $document->commitPoint("One"); // acquittement des requêtes depuis le point "One"
    // le document en BD a maintenant comme nom "Test One"
    // ATTENTION : le document conserve en mémoire sa dernière valeur "Test two".

Les points de sauvegarde ne sont pas liés à un objet mais impactent toutes les
requêtes liées à la base.

Si vous avez déclaré un savePoint il faut obligatoirement avoir un commitPoint
ou un rollbackPoint de ce point. Dans le cas contraire toutes les modifications
sur la base de données, depuis le premier point de sauvegarde, sont
abandonnées. Les méthodes de point de sauvegarde sont accessibles depuis tout
objet dbObj, pas seulement Doc.

Néanmoins, il n'est pas obligatoire d'appliquer un commit ou un rollback sur tous
les points, mais il est obligatoire de le faire au moins sur le premier.

    [php]
    $document->savePoint("One");
    $document->setValue("us_lname","Test one");
    $document->store();
    
    $document->savePoint("Two");
    $document->setValue("us_lname","Test two");
    $document->store();
    
    $document->savePoint("Three");
    $document->setValue("us_lname","Test three");
    $document->store();
    $document->rollbackPoint("Two"); // on retourne au point Two, le point Three est effacé
    
    $document->commitPoint("One"); 

le document a maintenant comme nom "Test One" 

<!-- links -->

[transactionSQL]:       https://fr.wikipedia.org/wiki/Transaction_(base_de_donn%C3%A9es) "Wikipedia : Transaction"
[commitSQL]:            https://fr.wikipedia.org/wiki/Commit "Wikipedia : Commit"
[RollbackSQL]:          https://fr.wikipedia.org/wiki/Rollback_(base_de_donn%C3%A9es) "Wikipedia : Rollback"