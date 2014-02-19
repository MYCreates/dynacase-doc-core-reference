# SearchDoc::returnsOnly()  {#core-ref:6429b289-8fec-4c7e-8906-5f367c5bf59d}

<div class="short-description" markdown="1">

Cette méthode permet d'indiquer quels sont les [propriétés][docprop] ou
[attributs][docattr] que doit retourner la recherche.

</div>


## Description  {#core-ref:6fd78d70-b7fd-420d-bb7a-d98312f149ab}

    [php]
    void returnsOnly (array $returns)

Cette méthode permet d'accélérer le traitement de la requête en indiquant un
sous-ensemble d'attributs ou de propriétés. Ceci a deux avantages :

1.  Moins de données transférées entre la base de données et le serveur
2.  Moins de consommation mémoire

L'inconvénient est que les documents retournés ne sont pas complets. Ceci
implique que **ces documents ne peuvent faire l'objet de modification**.

### Avertissements  {#core-ref:1eeba17b-606c-45e0-8d36-d75593962e1c}

Si le mode est [`retour d'objet documentaire`][setobjectreturn], les documents
retournés sont marqués *"incomplet"* (`doctype='I'`). Ils ne peuvent pas être
modifiés par la méthode [`Doc::store()`][docstore].

## Liste des paramètres  {#core-ref:e261638f-9d74-46d3-aea6-b21da41726d0}

(array) `returns` 
:   Indique une liste de propriétés ou d'attribut à récupérer.
    
    Si `returns` est vide, les 4 propriétés *élémentaires* du document sont
    retournées :
    
    -   id
    -   title
    -   fromid
    -   doctype

## Valeur de retour  {#core-ref:5e181a94-0890-4131-a7b9-68669ac15e9e}

void

## Erreurs / Exceptions  {#core-ref:05572c2e-3fb8-44c0-bf7d-79ce95feebd4}

Aucune.


## Historique  {#core-ref:edf76c63-6b66-4137-93a4-d545c155bb97}

Aucun.


## Exemples  {#core-ref:1c4b7489-0f78-41bf-9788-d83f62e88da1}

### Retour normal

    [php]
    $s=new SearchDoc("","ZOO_ANIMAL");
    $s->setObjectReturn(true);
    $s->setOrder('initid');
    $s->search();
    
    printf("Requête : %s\n",print_r($s->getSearchInfo(), true));
    
    print("Résultats :\n");
    $documentList=$s->getDocumentList();
    foreach ($documentList as $docid=>$doc) {
      printf("%d) %-10s : %s\n", 
         $docid,
         $doc->getTitle(),
         $doc->getRawValue(\Dcp\AttributeIdentifiers\Zoo_Animal::an_espece));
    }

La partie *"select"* contient toutes les propriétés et tous les attributs de
la famille.

Résultat :

    Requête : Array
    (
        [count] => 3
<!--beware there is no tab here for sql syntax-->

    [sql]
        [query] => select family."zoo_animal".*  from  family."zoo_animal" where  (family."zoo_animal".archiveid is null) and (family."zoo_animal".doctype != 'T') and (family."zoo_animal".locked != -1) and (views && '{2,0,172}') ORDER BY initid LIMIT ALL OFFSET 0;
        
        select family."zoo_animal".*  
        from  family."zoo_animal" 
        where  (family."zoo_animal".archiveid is null) 
           and (family."zoo_animal".doctype != 'T') 
           and (family."zoo_animal".locked != -1) 
           and (views && '{2,0,172}') 
        ORDER BY initid LIMIT ALL OFFSET 0;select family."zoo_animal".*  from  family."zoo_animal" where  (family."zoo_animal".archiveid is null) and (family."zoo_animal".doctype != 'T') and (family."zoo_animal".locked != -1) ORDER BY initid LIMIT ALL OFFSET 0;
        [error] => 
        [delay] => 0.017s
    )

    Résultats :
    1419) Rotor      : 1295
    1420) Théodor    : 1295
    1421) Éléonore   : 1295

## Retour minimaliste

Dans cet exemple, seules les quatre propriétés nécessaires sont retournées.

    [php]
    $s=new SearchDoc("","ZOO_ANIMAL");
    $s->setObjectReturn(true);
    $s->returnsOnly(array());
    printf("Retour : %s\n",print_r($s->getReturnsFields(), true));
    $s->setOrder('initid');
    $s->search();
    
    printf("Requête : %s\n",print_r($s->getSearchInfo(), true));
    
    print("Résultats :\n");
    $documentList=$s->getDocumentList();
    foreach ($documentList as $docid=>$doc) {
      printf("%d) %-10s : %s\n", 
         $docid,
         $doc->getTitle(),
         $doc->getRawValue(\Dcp\AttributeIdentifiers\Zoo_Animal::an_espece));
    }

Résultat :

    Retour : Array
    (
        [0] => id
        [1] => title
        [2] => fromid
        [3] => doctype
    )

&nbsp;

    Requête : Array
    (
        [count] => 3
<!--beware there is no tab here for sql syntax-->

    [sql]
        [query] => select family."zoo_animal".id, title, fromid, doctype  
                   from  family."zoo_animal" 
                   where  (family."zoo_animal".archiveid is null) 
                     and (family."zoo_animal".doctype != 'T') 
                     and (family."zoo_animal".locked != -1) 
                     and (views && '{2,0,172}') 
                   ORDER BY initid LIMIT ALL OFFSET 0;
        [error] => 
        [delay] => 0.003s
    )

    Résultats :
    1419) Rotor      : 
    1420) Théodor    : 
    1421) Éléonore   : 

**Note** : Puisque `an_espece` n'est pas demandé par `returnsOnly`, sa valeur
n'est pas disponible.

### Retour spécifique

    [php]
    $s=new SearchDoc("","ZOO_ANIMAL");
    $s->setObjectReturn(true);
    $s->returnsOnly(array(
        'locked',
        \Dcp\AttributeIdentifiers\Zoo_Animal::an_classe,
        \Dcp\AttributeIdentifiers\Zoo_Animal::an_espece
    ));
    printf("Retour : %s\n",print_r($s->getReturnsFields(), true));
    $s->setOrder('initid');
    $s->search();
    
    printf("Requête : %s\n",print_r($s->getSearchInfo(), true));
    
    print("Résultats :\n");
    $documentList=$s->getDocumentList();
    foreach ($documentList as $docid=>$doc) {
      printf("%d) %-10s : [lock uid : %d] %d/%d\n", 
         $docid,
         $doc->getTitle(),
         $doc->getProperty('locked'),
         $doc->getRawValue(\Dcp\AttributeIdentifiers\Zoo_Animal::an_classe),
         $doc->getRawValue(\Dcp\AttributeIdentifiers\Zoo_Animal::an_espece));
    }

Résultat :

    Retour : Array
    (
        [0] => id
        [1] => title
        [2] => fromid
        [3] => doctype
        [4] => locked
        [5] => an_classe
        [6] => an_espece
    )

&nbsp;

    Requête : Array
    (
<!--beware there is no tab here for sql syntax-->

    [sql]
        [query] => SELECT family."zoo_animal".id, title, fromid, doctype, locked, an_classe, an_espece
                   from  family."zoo_animal" 
                   where  (family."zoo_animal".archiveid is null) 
                     and (family."zoo_animal".doctype != 'T') 
                     and (family."zoo_animal".locked != -1) 
                     and (views && '{2,0,172}') 
                   ORDER BY initid LIMIT ALL OFFSET 0;
        [error] => 
        [delay] => 0.003s
    )

    Résultats :
    1419) Rotor      : [lock uid : 0]  1291/1295
    1420) Théodor    : [lock uid : 11] 1291/1295
    1421) Éléonore   : [lock uid : 0]  1291/1295


Dans ces exemples, le temps 

## Notes  {#core-ref:de48a535-5320-49b9-a2c2-17ce0c97b298}

Les quatre [propriétés][docprop]: suivantes sont toujours retournées :

*   `id`
*   `initid`
*   `doctype`
*   `fromid`

Les champs ([propriétés][docprop] ou [attributs][docattr]) non conforme à la
famille sont ignorés. Si la recherche ne porte pas sur une famille spécifique,
seules les [propriétés][docprop] peuvent être indiquées dans le paramètre.

La gain de performance est d'autant plus grand que le nombre d'éléments est
retourné est important.

Exemple : récupération de tous les documents sur une base de 55084 documents volumineux :

Avec un retour "normal" : **&gt; 6 min**

    Requête : Array
    (
        [count] => 55084
<!--beware there is no tab here for sql syntax-->

    [sql]
        [query] => SELECT docread.*
                   FROM docread
                   WHERE (docread.archiveid is null) AND (docread.doctype != 'Z') AND (docread.doctype != 'T') AND (docread.locked != -1) 
                   ORDER BY initid LIMIT ALL OFFSET 0;

        [error] => 
        [delay] => 367.984s
    )


Avec un retour "minimaliste" : **&lt; 200ms**

    Requête : Array
    (
        [count] => 55084
<!--beware there is no tab here for sql syntax-->

    [sql]
        [query] => SELECT docread.id, title, fromid, doctype
                   FROM docread
                   WHERE (docread.archiveid is null) AND (docread.doctype != 'Z') AND (docread.doctype != 'T') AND (docread.locked != -1)
                   ORDER BY initid LIMIT ALL OFFSET 0;

        [error] => 
        [delay] => 0.192s
    )



## Voir aussi  {#core-ref:0b823e98-a011-4a1d-8d6d-fbebd06b26e6}

*   [`SearchDoc::search()`][searchdocsearch]
*   [`FormatCollection`][formatcol]


<!-- links -->

[SearchDoc]:        #core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255
[setobjectreturn]:  #core-ref:3a0b4882-81ff-4030-9f60-a0ed0ff1f958
[docstore]:         #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[docprop]:          #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353
[docattr]:          #core-ref:4e167170-33ed-11e2-8134-a7f43955d6f3
[searchdocsearch]:  #core-ref:6f5cc024-66e4-429e-9071-67d4523a8e08
[formatcol]:        #core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81