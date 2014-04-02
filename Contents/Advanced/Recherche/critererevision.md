# Recherche sur les propriétés du document {#core-ref:1e7cbc19-65cf-436c-b27d-75e9853adc5a}

## Recherche et révision {#core-ref:cbb4116f-f49b-413b-b9a9-1dc17668fc57}

Par défaut seule la dernière révision du document est retournée. Pour chercher
sur l'ensemble des révisions, il faut mettre la propriété `latest` a `false`.

    [php]
    $s=new SearchDoc(getDbAccess(), $familyId);
    $s->setObjectReturn(true);
    $s->latest=false; // toutes les révisions

Si la propriété `locked = -1`, cela indique que le document est figé.

## Recherche sur les propriétés {#core-ref:c6cc1dd2-0c6e-4665-9221-d50f569813ce}

Cet exemple montre la recherche de document par le titre. Ici, tout type de
document est retourné si son titre contient 'jean' en majuscule ou minuscule.
Les opérateurs utilisables sont les [opérateurs SQL][pgop] de PostgreSQL.

    [php]
    $nom="jean";
    $s=new SearchDoc();// famid : toute famille
    $s->addFilter("title ~* '%s'",$nom); // titre contient jean 
    $s->setObjectReturn();
    $dl=$s->search()->getDocumentList();
     
    foreach ($dl as $k=>$doc)
        print "$k)".$doc->getTitle()."\n";
    }

Dans la cas où on recherche une valeurs dans n'importe quel attribut du
document, on utilisera la [propriété][propdoc] `svalues`. Cet attribut contient
la concaténation des valeurs des autres attributs séparés par le caractère `£`.

    [php]
    $searchExpression='myWord';
    $s=new SearchDoc();// famid : toute famille
    $s->addSearchFilter("svalues ~* '%s'", $searchExpression); 


## Recherche sur les états d'un document lié à un cycle de vie {#core-ref:1b9c384a-bd2d-445b-9232-f07d59788482}

La recherche des documents en fonction de leur activité ou de leur état est
faite en filtrant sur la propriété `state`. La clef correspondante est
l'identifiant indiqué dans les propriétés `e1` et `e2` du [cycle][states].

La recherche suivant l'activité est forcément effectuée sur les dernières révisions.

    [php]
    /**
     * @var WDoc $wdoc
     */
    $wdoc=\Dcp\DocManager::getDocument("ZOO_CYCLEDA" ); // Cycle sur les demandes d'adoption
    $s=new SearchDoc("", "ZOO_DEMANDEADOPTION"); // Famille demande d'adoption
    $s->addFilter("state = '%s'", $wdoc->getFirstState()); // recherche des document étant dans la première activité
    $s->search();

La recherche sur l'état est effectuée sur les révisions passées.

    [php]
    $s=new SearchDoc("", "ZOO_DEMANDEADOPTION"); // Famille demande d'adoption
    $s->addFilter("state = '%s'", \Zoo\WDemandeAdoption::submittedState); // recherche des document ayant été soumis
    $s->latest=false; // toutes les révisions
    $s->addFilter("locked = -1"); // révisions passés uniquement
    $s->search();

<!-- link -->
[searchdoc]:        #core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255
[propdoc]:          #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353 "Liste des propriétés du document"
[layoutblock]:      #core-ref:587b563e-7371-469f-9d1e-350607056c73
[formatcollection]: #core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81
[pgop]:             http://www.postgresql.org/docs/9.1/static/functions.html "Opérateurs Postgresql 9.1"
[docattributs]:     #core-ref:4e167170-33ed-11e2-8134-a7f43955d6f3
[attdocid]:         #core-ref:d461d5f5-b635-47a0-944d-473c227587ab
[phpiterator]:      http://php.net/manual/fr/class.iterator.php "Interface Iterator"
[docacl]:           #core-ref:a99dcc5f-f42f-4574-bbfa-d7bb0573c95d "Droits du document"
[states]:           #core-ref:d5ddda0c-09d2-42b0-9543-0723e242ec09