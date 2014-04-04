# Doc::getRawValue() {#core-ref:f779391c-ee61-4c3a-8976-6b74f83ecc8f}

<div class="short-description">
Récupère la valeur brute d'un attribut de document.
</div>

## Description {#core-ref:c1dd1c53-6c29-4ec7-a39e-8c568ea00c96}

    [php]
    string getRawValue ( string $idAttribute, mixed $defaultValue = "" )

Cette méthode retourne la valeur d'un attribut telle qu'elle est inscrite en
base de données. Le type retourné est toujours une chaîne de caractères quel que
soit le type d'attribut.

### Avertissements {#core-ref:dfe696a3-1202-4829-a8f8-160d00583ae5}

Aucun.

## Liste des paramètres {#core-ref:4c2adc5a-0b68-41bc-b411-8f44ce10799d}

(string) `idAttribute`
:   Identifiant de l'attribut

(string) `defaultValue`
:   Valeur par défaut à retourner si la valeur est vide ou si l'attribut
    n'existe pas

## Valeur de retour {#core-ref:f442d952-7de6-4cbe-9391-342b8d89cbaf}

Si l'attribut du document existe et s'il n'est pas vide, sa valeur brute est
retournée.

Cette méthode retourne tout attribut de la classe sans vérification. Si cet
attribut est non existant ou vide alors c'est la valeur par défaut qui sera
retournée.

## Erreurs / Exceptions {#core-ref:31f702b0-5735-4e76-998a-c0233bcf5db0}

Aucune erreur retournée.

## Historique {#core-ref:edb5cb1c-3388-457d-a0ab-7450fc95fcc9}

Cette méthode était anciennement nommée `getValue`.

### Version 3.3.0

Modification des retours pour les valeurs des attributs multivalués (voir
[note][inote]).

## Exemples {#core-ref:0134ab3f-26b4-4826-af8a-183786c18460}

Soit la famille suivante :

| BEGIN |                   | Ma famille        |                 |     | MYFAMILY |       |     |     |   |                                     |     |
| ----- | ----------------- | ----------------- | --------------- | --- | -------- | ----- | --- | --- | - | ----------------------------------- | --- |
| CLASS | My\MyFamily       |                   |                 |     |          |       |     |     |   |                                     |     |
| //    | idattr            | idframe           | label           | T   | A        | type  | ord | vis | … | phpfunc                             |     |
| ATTR  | MY_IDENTIFICATION |                   | Identification  | N   | N        | frame | 10  | W   |   |                                     |     |
| ATTR  | MY_NUMBERONE      | MY_IDENTIFICATION | nombre 1        | Y   | N        | int   | 20  | W   |   |                                     |     |
| ATTR  | MY_NUMBERTWO      | MY_IDENTIFICATION | nombre 2        | N   | N        | int   | 30  | W   |   |                                     |     |
| ATTR  | MY_SUM            | MY_IDENTIFICATION | nombre 1&plus;2 | N   | N        | int   | 30  | R   |   | ::mySum(MY_NUMBERONE, MY_NUMBERTWO) |     |
| END   |                   |                   |                 |     |          |       |     |     |   |                                     |     |

Utilisation des valeurs de `my_numberone` et `my_numbertwo` pour la méthode
`mySum` de l'[attribut calculé][computeattr] `my_sum`.

Avec la classe :

    [php]
    namespace My;
    use \Dcp\AttributeIdentifiers\MyFamily as MyAttributes;
    
    class MyFamily extends \Dcp\Family\Document
    {
        public function mySum($x, $y)
        {
            $n1 = intval($this->getRawValue(MyAttributes::my_numberone));
            // si "my_number_two" est vide, alors, la valeur sera 543
            $n2 = intval($this->getRawValue(MyAttributes::my_numbertwo, "543"));

            return ($n1 + $n2);
        }
    }


Vérification de l'attribut `my_sum` en fonction des valeurs de `my_numberone` et
`my_numbertwo` 

    [php]
    use \Dcp\AttributeIdentifiers\MyFamily as Attributes\MyFamily;
    /** @var \Dcp\Family\MyFamily */
    $myDoc = \Dcp\DocManager::getDocument( "MY_DOCUMENT");
    if ($myDoc->isAlive()) {
        $n1=intval($myDoc->getRawValue(Attributes\MyFamily::my_numberone));
        $n2=intval($myDoc->getRawValue(Attributes\MyFamily::my_numbertwo, "543"));
        $sum=intval($myDoc->getRawValue(Attributes\MyFamily::my_sum));
        if ($sum != ($n1 +$n2)) {
            printf("La somme est incorrecte : %d + %d <> %d ! \n", $n1, $n2, $sum);
        }
    }

## Notes {#core-ref:6302d6cf-bbd1-43ec-a74c-2537581d051c}

Format brut en fonction des types d'attributs :

`date` 
:   les dates sont retournées sous la forme "YYYY-MM-DD".

`timestamp` 
:   les horo-dates sont retournées sous la forme "YYYY-MM-DD HH:MM:SS".

`time`
:   les temps sont retournés sous la forme "HH:MM:SS".

`file`, `image`
:   les pointeurs de fichiers sous la forme "[mimeType]|[vaultId]|[fileName]"

`htmltext` 
:   le texte retourné est un fragment html dont les entités html
    (caractères accentués notamment) ont été convertis en caractères utf-8.

Pour les autres types, aucun formatage spécial n'est appliqué, la valeur brute
correspond à la valeur donnée.


Pour les valeurs multiples, chaque valeur est séparée par une virgule dans une
[structure avec accolades][pgarray].

Exemple : `{Hello, World, "Good day isn't it ?"}`

indique 3 valeurs qui sont respectivement :

1.  Hello
2.  World
3.  Good day isn't it ?



Pour les multiples à 2 niveaux (attribut multiple dans un tableau), les
accolades sont imbriquées. 

**Note** : pour des raisons de contrainte de type, les tableaux à deux dimensions
sont de dimension fixe. Le nombre de chacune des sous-valeurs est identique, la
valeur "NULL" est ajoutée pour satisfaire la contrainte

Exemple : "`{{1234,NULL,NULL},{567,8876,987},{678,295,NULL}}`"
indique la structure à 2 niveaux suivante :

1.   -
    1. 1234
2.  -
    1. 567
    1. 8876
    1. 987
1.  -
    1. 678
    1. 295 



## Voir aussi {#core-ref:78ce65c8-e7f6-4578-8b6a-3f825db4adbe}

*   [`Doc::setValue()`][docsetvalue],
*   [`Doc::getAttributeValue()`][docgetattrvalue],
*   [`Doc::getOldValue()`][docgetoldvalue].

<!-- links -->
[docgetattrvalue]:  #core-ref:e4a8d6ff-7229-4105-81c4-94773ac24dfd
[docgetrawvalue]:   #core-ref:f779391c-ee61-4c3a-8976-6b74f83ecc8f
[docgetoldvalue]:   #core-ref:dccf7c64-8f4f-4c4a-8d0d-79b21b924848
[docsetvalue]:      #core-ref:febc397f-e629-4d47-955d-27cab8f4ed2f
[computeattr]:      #core-ref:4565cab9-73c8-4eee-bfa7-218ffbd4b687
[pgarray]:          http://www.postgresql.org/docs/9.3/static/arrays.html#ARRAYS-INPUT "Notation des tableaux postgreSql"
[inote]:    #core-ref:6302d6cf-bbd1-43ec-a74c-2537581d051c