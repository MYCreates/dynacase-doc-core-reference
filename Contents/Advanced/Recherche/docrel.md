# Recherche des relations d'un document {#core-ref:fb23820a-e8e9-47a0-a22f-d25458e51ed0}

Pour trouver les documents qui sont lié à un autre, il faut utiliser la classe `DocRel`.
La méthode `DocRel::getIRelations()` donne la liste des documents liés vers un document.

    [php]
    include_once("FDL/Class.Doc.php");
    $docrelation=new DocRel();
    $doc=\Dcp\DocManager::getDocument( 5835 , true);
    
    $docrelation=new DocRel($doc->initid);
    
    $toMeRelation=$docrelation->getIRelations();
    $fromMeRelation=$docrelation->getRelations();
    
    print "\n# --------- Relation vers moi\n";
    print_r($toMeRelation);
    print "\n# --------- Relation depuis moi\n";
    print_r($fromMeRelation);

La méthode `DocRel::getIRelations()` retourne les caractéristiques des documents liés.

*   `sinitid` : identifiant initial source (celui qui pointe vers la cible),
*   `cinitid` : identifiant initial cible (la cible pointé par la source),
*   `stitle` : titre de la source,
*   `ctitle` : titre de la cible,
*   `sicon` : icone de la source,
*   `cicon` : icone de la cible,
*   `type` : type de lien (nom de l'attribut ayant établi le lien),
*   `doctype` : doctype du document source.

Résultat :

 <table>
    <tr><td style="vertical-align:top;width:50%"><pre>
    # --------- Relation vers moi
    Array
    (
        [0] => Array
            (
                ["sinitid"] => 5973
                ["cinitid"] => 5835
                ["ctitle"] => "Dogue Robert"
                ["cicon"] => "ugarde.png"
                ["stitle"] => "Théodor"
                ["sicon"] => "animal.png"
                ["type"] => "an_gardien"
                ["doctype"] => "F"
            )
        [1] => Array
            (
                ["sinitid"] => 5835
                ["cinitid"] => 5835
                ["ctitle"] => "Dogue Robert"
                ["cicon"] => "ugarde.png"
                ["stitle"] => "Dogue Robert"
                ["sicon"] => "ugarde.png"
                ["type"] => "us_meid"
                ["doctype"] => "F"
            )
    )
 </pre></td><td ><pre>
    # --------- Relation depuis moi
    Array
    (
        [0] => Array
            (
                ["sinitid"] => 5835
                ["cinitid"] => 5830
                ["ctitle"] => "Surveillants"
                ["cicon"] => "igroup.png"
                ["stitle"] => "Dogue Robert"
                ["sicon"] => "ugarde.png"
                ["type"] => "us_idgroup"
                ["doctype"] => "F"
            )
        [1] => Array
            (
                ["sinitid"] => 5835
                ["cinitid"] => 1010
                ["ctitle"] => "Utilisateurs"
                ["cicon"] => "igroup.png"
                ["stitle"] => "Dogue Robert"
                ["sicon"] => "ugarde.png"
                ["type"] => "us_idgroup"
                ["doctype"] => "F"
            )
        [2] => Array
            (
                ["sinitid"] => 5835
                ["cinitid"] => 5832
                ["ctitle"] => "Gardien surveillant"
                ["cicon"] => "role.png"
                ["stitle"] => "Dogue Robert"
                ["sicon"] => "ugarde.png"
                ["type"] => "us_roles"
                ["doctype"] => "F"
            )
 </pre></td></tr></table>

Pour exploiter les résultats, il est possible d'utiliser un objet `DocumentList`.

    [php]
    $ids=array_map(
        function ($r) {
            return $r["sinitid"];
        },
        $toMeRelation
    );
    $dl=new DocumentList();
    $dl->addDocumentIdentifiers($ids);
    /**
     * @var Doc $doc
     */
    foreach ($dl as $doc) {
        printf("%s %s '%d'\n", $doc->getFamilyDocument()->getTitle(), $doc->getTitle(), $doc->id);
    }

Résultat :

    Gardien Dogue Robert '5835'
    animal Théodor '5973'

L'objet `$dl` contient alors les documents pointant vers la cible.

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