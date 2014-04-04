# Les dossiers {#core-ref:aa28a2c3-e74d-4b36-ad8f-ae1bb93b6726}

Les dossiers permettent de grouper des documents. Un dossier est un type de
[collection][collection], comme les [recherches][Search] et [rapport][report].

Cette fonction est analogue à la fonction de [répertoires][repertoire]
présents dans les systèmes de fichiers mais présente de notables différences.

Un dossier ne gère que des références à des documents. Au
contraire du répertoire qui, dans un système de fichiers, impose que les
fichiers soient liés *physiquement* à un répertoire. Cela implique les faits
suivants :

*   la suppression d'un dossier n'entraîne pas la suppression de ses références,
*   l'accès à un dossier ne contraint pas l'accès à une des ses références,
*   les références entre dossiers peuvent conduire à des boucles dans l'arborescence,
*   un document ou un dossier peut être référencé dans aucun, un seul ou plusieurs dossiers.

Par défaut, n'importe quelle famille de document peut être insérée dans un
dossier. Un dossier peut être paramétré pour ne contenir que certaines familles
de documents, c'est ce qu'on nomme la [restriction][restriction].

## Restrictions {#core-ref:ad55c0a7-fc0f-4c9d-95cb-8286f4057c3f}

La restriction d'un dossier est paramétrée à l'aide des attributs :

*   [`FLD_ALLBUT`][fld_allbut]
*   [`FLD_FAMIDS`][fld_famids]

### Attribut FLD_ALLBUT {#core-ref:9d1e4708-439b-484c-bb1f-a80031e6a20a}

L'attribut [`FLD_ALLBUT`][fld_allbut] est un énuméré qui peut prendre deux
valeurs (`0` ou `1`) et qui permet de spécifier le mode de restriction :

`0` (tout sauf)
:   Autorise l'insertion de toutes les familles de documents à l'exception d'une liste
    déterminée de famille à exclure.

`1` (seulement)
:   N'autorise que l'insertion de documents des familles déterminées.

### FDL_FAMIDS {#core-ref:61ece13c-6fbd-4e1b-a7f3-3b1d7bae264c}

L'attribut `FLD_FAMIDS` est un attribut multivalué qui contient
des identifiants de familles, et qui permet de spécifier la liste des familles à
autoriser ou à exclure (en fonction de la valeur de l'attribut
[`FDL_ALLBUT`][fld_allbut]).

L'attribut `FLD_SUBFAM` est un attribut multivalué qui indique si la famille
indiquée dans la restriction `FLD_FAMIDS` concerne aussi les sous-famille ou non.
Les valeurs possible de `FLD_SUBFAM` sont :

*   `yes` : avec les sous-familles,
*   `no` : sans les sous-familles.

### Exemple de paramétrage de restriction {#core-ref:a4fcc391-2e6c-453d-ae1e-2eeef88d00d8}

N'autoriser que l'insertion de documents issus des familles `FAM_A` (et ses
dérivées) et `FAM_B` (sans ses dérivées) dans le dossier `My_FOLDER`.

    [php]
    /**
     * @var Dir $myFolder
     */
    $myFolder =  \Dcp\DocManager::getDocument("My_FOLDER");
      
    $idFamA = getIdFromName(getDbAccess(), 'FAM_A');
    $idFamB = getIdFromName(getDbAccess(), 'FAM_B');
    
    $myFolder->setAttributeValue(\Dcp\AttributeIdentifiers\Dir::fld_allbut, 1);
    $myFolder->setAttributeValue(\Dcp\AttributeIdentifiers\Dir::fld_famids, array($idFamA, $idFamB));
    $myFolder->setAttributeValue(\Dcp\AttributeIdentifiers\Dir::fld_subfam, array("yes","no"));
    $myFolder->store();


## Profil de dossier {#core-ref:0cd3fe9a-57cf-481f-8fc0-560bc71d6430}

Le [profil][profil] de dossier hérite du profil de document.

La famille `PDIR` est le profil de dossier. En plus des droits du document
"classique", deux droits spécifiques sont ajoutés :

*   `open` : Permet d'accéder au contenu du dossier.
*   `modify` : Permet d'[ajouter][insertdocument] ou de 
    [supprimer][removedocument] des références au dossier.

**Note** : Le droit `edit` n'est pas utilisé pour vérifier la modification du
contenu mais il est utilisé pour vérifier la modification des informations sur
le document dossier tel que son titre et sa description.

## Profil de contenu {#core-ref:a14f99fc-eb7f-4930-b282-857649690dd1}

En terme de droit, le dossier possède une fonctionnalité spécifique.
Il peut modifier les droits des documents qui sont insérés dans le dossier.

L'attribut `fld_pdocid` et `fld_pdirid` permettent d'indiquer le profil qu'
acquiert un document lorsqu'il est inséré dans le dossier.

*   `fld_pdocid` : profil utilisé pour les documents "classique"
*   `fld_pdirid` : profil utilisé pour les documents "dossier"

**Attention** : Le profil d'un document inséré est modifié seulement s'il n'est
pas déjà profilé (`profid = 0`) et seulement lors d'une 
[insertion unitaire][insertdocument] et non lors d'une 
[insertion multiple][insertmultiple].

## Hook (hameçons) propres aux documents dossiers {#core-ref:093147f8-f0a6-4d9a-a016-fe6b77d79787}

La modification d'un profil de document inséré peut être personnalisée en
utilisant les hameçons (hook) [`Dir::postInsertDocument()`][postinsertdoc] et
[`postInsertMultipleDocuments()`][postinsertdocs].

## Voir aussi {#core-ref:19c8e77e-ba1b-4deb-bb6d-f52670284773}

Voir la classe [`Dir`][dir] qui contient les méthodes spécifiques aux dossiers.

<!-- links -->

[repertoire]:       http://fr.wikipedia.org/wiki/R%C3%A9pertoire_%28informatique%29 "Répertoire sur Wikipédia"
[restriction]:      #core-ref:ad55c0a7-fc0f-4c9d-95cb-8286f4057c3f
[fld_allbut]:       #core-ref:9d1e4708-439b-484c-bb1f-a80031e6a20a
[fld_famids]:       #core-ref:61ece13c-6fbd-4e1b-a7f3-3b1d7bae264c
[dir]:              #core-ref:dd2077e0-7f3d-4974-bdb0-62a92427754d
[profil]:           #core-ref:f1575705-10e8-4bf2-83b3-4c0b5bfb77cf
[insertdocument]:   #core-ref:9575ff95-480a-4dfb-9cd0-b89f44c3fad7
[insertmultiple]:   #core-ref:b66ef951-c5ee-4ee0-9499-7913ed805042
[removedocument]:   #core-ref:d337e186-8066-49e2-92a0-26aa518cbf41
[postinsertdoc]:    #core-ref:65ec2b4a-8878-4004-8e42-0de8c359a231
[postinsertdocs]:   #core-ref:e3cd509f-8678-4dec-a0cf-33aa39674cfe
[Search]:           #core-ref:db2507b5-cfc9-4d2b-87da-a29914223194
[report]:           #core-ref:4d6429f1-b3ae-46c4-8687-1c8d29f92f37
[collection]:       #core-ref:a2c4bd53-c31f-4448-82e4-7ec1d2f7f346
