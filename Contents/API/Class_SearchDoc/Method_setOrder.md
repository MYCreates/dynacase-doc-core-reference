# SearchDoc::setOrder() {#core-ref:d4f0b749-ed89-460e-a84c-6c044cb1c7d7}

<div class="short-description">
Les résultats de la recherche peuvent être ordonnés à l'aide de la méthode
"setOrder()" qui permet de spécifier les attributs en fonction desquels seront
triés les résultats.
</div>


## Description {#core-ref:051c8c96-edad-492e-92a5-4f492ea8fb14}

    [php]
    void setOrder ( string $order, 
                    string $orderbyLabel = '' )

### Avertissements {#core-ref:f596b417-1d51-4b8a-9308-2aec7152c27c}

Cette fonction ne produit pas de message d'erreurs si les attributs passés ne 
sont pas valides.

## Liste des paramètres {#core-ref:d6cc0c54-2a69-4dce-b1a3-3879ff139e17}

(string) `order` 
:    une chaîne de caractères contenant la liste des colonnes (séparées par une
    virgule) en fonction desquelles le résultat est ordonné. Le nom des 
    colonnes peut être suffixé par un espace et le mot-clef 'asc' ou 'desc' 
    afin de spécifier l'ordre du tri : ASCendant ou DESCendant (par défaut, la 
    colonne est triée dans l'ordre ASCendant).

(string) `orderbyLabel` 
:   une chaîne de caractères contenant le nom d'une des colonnes spécifiées dans
    l'argument #1 (sans le suffixe de tri 'asc' ou 'desc') et pour laquelle le 
    tri doit être fait non pas sur la valeur de l'attribut, mais sur le label 
    ou le titre. Les attributs actuellement supportés pour l'ordonnancement par 
    le label ou le titre sont : Les attributs de type [`enum`][attrenum].  
    Les attributs de type 'docid("X")' déclarés avec une option 
    'doctitle=auto' ou 'doctitle=xxx'.  
    Pour les énumérés, les libellés peuvent être différents suivant la 
    [locale][i18nenum]. L'ordre est alors différent suivant la langue choisie 
    lors de la connexion de l'utilisateur.

## Valeur de retour {#core-ref:d8e71435-38f9-44b5-9615-b8abb5d875f6}

Aucune.

## Erreurs / Exceptions {#core-ref:fa716683-f56e-4535-854c-d30a7f42ef83}

Aucune.

## Historique {#core-ref:c3158295-d9f3-489c-88e6-04ff104b4608}

Aucun.

## Exemples {#core-ref:5bd8f543-e92e-4396-9720-70d6d8587cec}

### Exemple de tri en fonction d'un attribut entier : {#core-ref:5db9097b-2e41-442d-86f8-a3ffaee5becd}

    [php]
    $searchDoc=new searchDoc($dbaccess, $famId);
    $searchDoc->setOrder("a_integer desc");
    $searchDoc->search();
    

### Exemple de tri en fonction de deux attributs : {#core-ref:24dd06af-a25c-4620-8273-2f114843c33c}

    [php]
    $searchDoc=new searchDoc($dbaccess, $famId);
    $searchDoc->setOrder("a_integer desc, b_date asc");
    $searchDoc->search();

### Exemple de tri en fonction de la valeur d'un énuméré : {#core-ref:41e39a64-7d6a-4dd3-a548-4b63d92cedc2}

    [php]
    $searchDoc=new searchDoc($dbaccess, $famId);
    $searchDoc->setOrder("a_enum asc");
    $searchDoc->search();

### Exemple de tri en fonction du label d'un énuméré : {#core-ref:d1884fcf-0bfb-4cfd-8777-dd080c8144cb}

    [php]
    $searchDoc=new searchDoc($dbaccess, $famId);
    $searchDoc->setOrder("a_enum asc", "a_enum");
    $searchDoc->search();

Dans les exemples ci-dessus, si l'énuméré 'a_enum' est définit avec
`1|Accepté,2|Traité,3|Rejeté,4|Clos`, alors les documents sont retournés dans
l'ordre ci-dessous :


| setOrder("a_enum") | setOrder("a_enum", "a_enum") |
| ------------------ | ---------------------------- |
| 1-Accepté          | 1-Accepté                    |
| 2-Traité           | 4-Clos                       |
| 3-Rejeté           | 3-Rejeté                     |
| 4-Clos             | 2-Traité                     |


### Exemple de tri avec ou sans la fonction de `orderBy` : {#core-ref:58dc0b8a-2544-4f9b-99e8-d223576421aa}

L'énuméré `an_sexe` a comme définition : `M|Masculin,F|Zéminin,H|Hermaphorodite`.

    [php]
    function testOfOrderBy(Action & $action)
    {
        header('Content-Type: text/plain');
        
     
        $searchDoc = new searchDoc("", "ZOO_ANIMAL");
        $searchDoc->setObjectReturn();
        
        print "Without orderby\n";
        foreach ($searchDoc->getDocumentList() as $document) {
            printf("Title : %s \n\t sexe : %s \n", $document->getTitle(),
                 $document->getTextualAttrValue("an_sexe"));
        }
        
        var_export($searchDoc->getSearchInfo());
        print "\n***************\nOrder by without orderbyLabel\n";
        
        $searchDoc->setOrder("an_sexe");
        $searchDoc->reset();
        
        foreach ($searchDoc->getDocumentList() as $document) {
            printf("Title : %s \n\t sexe : %s \n", $document->getTitle(),
                 $document->getTextualAttrValue("an_sexe"));
        }
        
        var_export($searchDoc->getSearchInfo());
        print "\n***************\nOrder by with orderbyLabel\n";
        
        $searchDoc->setOrder("an_sexe", "an_sexe");
        $searchDoc->reset();
        
        foreach ($searchDoc->getDocumentList() as $document) {
            printf("Title : %s \n\t sexe : %s \n", $document->getTitle(),
                 $document->getTextualAttrValue("an_sexe"));
        }
        
        var_export($searchDoc->getSearchInfo());
        
    }

Résultat :

    Without orderby
    Title : Rotor 
         sexe : Masculin 
    Title : Séléonore 
         sexe : Zéminin 
    Title : Théodor 
         sexe : Masculin 
    array (
      'count' => 3,
      'query' => 'select family."zoo_animal".*  from  family."zoo_animal" where  (family."zoo_animal".archiveid is null) and (family."zoo_animal".doctype != \'T\') and (family."zoo_animal".locked != -1) ORDER BY title LIMIT ALL OFFSET 0;',
      'error' => '',
      'delay' => '0.005s',
    )
    ***************
    Order by without orderbyLabel
    Title : Séléonore 
         sexe : Zéminin 
    Title : Rotor 
         sexe : Masculin 
    Title : Théodor 
         sexe : Masculin 
    array (
      'count' => 3,
      'query' => 'select family."zoo_animal".*  from  family."zoo_animal" where  (family."zoo_animal".archiveid is null) and (family."zoo_animal".doctype != \'T\') and (family."zoo_animal".locked != -1) ORDER BY an_sexe LIMIT ALL OFFSET 0;',
      'error' => '',
      'delay' => '0.003s',
    )
    ***************
    Order by with orderbyLabel
    Title : Rotor 
         sexe : Masculin 
    Title : Théodor 
         sexe : Masculin 
    Title : Séléonore 
         sexe : Zéminin 
    array (
      'count' => 3,
      'query' => 'select family."zoo_animal".*  from  family."zoo_animal", (VALUES (\'M\', \'Masculin\'), (\'F\', \'Zéminin\'), (\'H\', \'Hermaphrodite\')) AS map_an_sexe(key, label) where (map_an_sexe.key = family."zoo_animal".an_sexe) and  (family."zoo_animal".archiveid is null) and (family."zoo_animal".doctype != \'T\') and (family."zoo_animal".locked != -1) ORDER BY map_an_sexe.label LIMIT ALL OFFSET 0;',
      'error' => '',
      'delay' => '0.002s',
    )

## Notes {#core-ref:bacd48d6-adf1-466f-9848-a01805179110}

Les colonnes de tri sont des noms d'attributs ou de propriétés de documents.

## Voir aussi {#core-ref:84c817d6-042d-4baf-bde4-4bb5fe81deb9}

*   [`SearchDoc::setPertinenceOrder`][pertineceorder]

<!-- links -->
[i18nenum]:       #core-ref:f5872ef4-4170-11e3-ba58-48f953959281
[attrenum]:       #core-ref:625303d6-e5d1-40c3-b91d-d1b2e7307f67
[pertineceorder]: #core-ref:287db32e-abb9-4ea3-a692-3dfded873b48
