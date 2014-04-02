# Recherche spécialisée {#core-ref:f5708494-27c1-4bc9-8f2d-03b2ce4eb31d}

Pour toute recherche non prévue en standard par l'interface, il est possible de
programmer des recherches spécifiques. Elles pourront ensuite être utilisées
comme une recherche “normale” depuis l'interface grâce à la famille “recherche
spécialisée”. Lorsque vous éditez une recherche spécialisée, vous devez
renseigner

*   le fichier php où se trouve la fonction de recherche.
    
    Ce fichier devra être dans le répertoire `EXTERNALS` du contexte.
    
*   Le nom de cette fonction.
    
    Cette fonction prend les 3 arguments suivants :
    
    *   start : start index pour la recherche,
    *   slice : nombre max d'éléments à retourner,
    *   userid : utilisateur courant.
    
    Des [arguments supplémentaires][additional_arguments] peuvent également être
    fournis.

Exemple : fichier `EXTERNALS/mytest.php` :


    [php]
    namespace My;
    /**
     * function use for specialised search
     * return all document tagged TOVIEWDOC by current user
     * 
     * @param int $start start cursor
     * @param int $slice offset ("ALL" means no limit)
     * @param int $userid user system identifier (NOT USED in this function)
     */
    function myToViewTags($start="0", $slice="ALL", $userid=0) {
        $tag="TOVIEWDOC";
        $s=new \SearchDoc();
        $s->setObjectReturn(false);
        $s->join("id = docutag(id)");
        $s->setSlice($slice);
        $s->setStart($start);
        $s->addFilter("docutag.uid = %d",$userid);
        $s->addFilter("docutag.tag = '%s'",$tag);
        
        return $s->search();
    }

## Enregistrement d'une recherche spécialisée {#core-ref:a01ca5da-6e87-4e2f-a1f6-a834a20670ca}

L'enregistrement se fait en créant un document de la famille `SSEARCH`. Les
attributs `se_phpfile` et `se_phpfunc` permettent d'indiquer la fonction à
utiliser.

    [php]
    use \Dcp\AttributeIdentifiers as Attribute;
    
    /**
     * @var \Dcp\Family\Ssearch $rd
     */
    $sd = \Dcp\DocManager::createDocument(\Dcp\Family\Ssearch::familyName);
    $sd->setValue(Attribute\Ssearch::ba_title, "Search TOVIEWDOC Tag");
    $sd->setValue(Attribute\Ssearch::se_phpfile, "mytest.php"); // EXTERNALS/mytest.php
    $sd->setValue(Attribute\Ssearch::se_phpfunc, "My\\myToViewTags");
    $sd->store();

## Arguments supplémentaires de la fonction {#core-ref:64e7b7b7-188f-4576-a49f-f36b32664162}

Des arguments supplémentaires peuvent être ajoutés dans l'attribut 'Argument
PHP' (`se_phparg`). Il sont ajoutés dans l'appel à partir de la quatrième
position. Pour rajouter plusieurs arguments, il faut les séparer par une virgule
(exemple : `1234,ceci est un test,dernier argument`).

Dans ces arguments, il est possible de référencer

*   N'importe quel attribut ou propriété du document recherche lui même, au
    moyen de la notation `%ATTRID%` ou `%PROPID%`.
    
    Par exemple, `%TITLE%` pour avoir le titre
    de la recherche, ou `%SE_IDCFLD%` pour avoir l'identifiant du dossier dans
    lequel s'exécute la recherche.

*   L'objet recherche lui-même au moyen du mot clé `%THIS%`.

## Retour de la fonction {#core-ref:14319d25-7f8d-44dc-80ea-68c58e233533}

La fonction de recherche doit retourner un tableau de *documents bruts* ( type
`array` et non `object`). Ce type est celui retourné par la classe `SearchDoc` en
utilisant la méthode `::setObjectReturn(false)`.

<!-- links -->
[additional_arguments]: #core-ref:64e7b7b7-188f-4576-a49f-f36b32664162
