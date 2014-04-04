# Doc::getAttributeValue() {#core-ref:e4a8d6ff-7229-4105-81c4-94773ac24dfd}

<div class="short-description">
Récupère la valeur typée d'un attribut de document.
</div>

## Description {#core-ref:628f94b9-d75d-4d9d-9372-bd7b119ca68a}

    [php]
    mixed getAttributeValue ( string $idAttribute )

Retourne la valeur d'un attribut du document.

### Avertissements {#core-ref:0ec2a298-1261-4ead-bab6-20e83e8fbb96}

Le retour de cette méthode peut être utilisé pour mettre à jour des valeurs avec
la méthode [Doc::setAttributeValue()][docsetattrvalue]. Par contre, ces valeurs
typées sont incompatibles avec la méthode [Doc::setValue()][docsetvalue] qui
prend en argument des valeurs brutes.

## Liste des paramètres {#core-ref:03d13d51-f0cb-4b95-a093-6cc853e60132}

[int] (string) `idAttribute`
:   Identifiant de l'attribut

## Valeur de retour {#core-ref:b2e45d57-63f8-408f-9779-ebf35e6967e4}

Retourne la valeur typée de l'attribut en fonction des types d'attributs :

`date`, `timestamp` 
:   Les dates sont retournées avec un objet de la classe
    [`DateTime`][phpdatetime] (classe interne de PHP).

`time`
:   Les temps sont retournés avec le type `string` sous la forme "HH:MM:SS".

`file`, `image`
:   Les pointeurs de fichiers sont retournés sous la forme
    "[mimeType]|[vaultId]|[fileName]".

`int`
:   Les nombres entiers sont retournés avec le type [`int`][phpint].

`double`, `money`
:   Les nombres décimaux sont retournés avec le type [`float`][phpdouble].

Les autres types sont retournés sous forme de chaîne de caractère (type
`string`).

Les valeurs multiples sont retournées avec un objet de type `array`. Les valeurs
multiples ayant aussi des valeurs multiples (cas des attributs multiples dans
les tableaux) sont retournés avec un objet de type `array[]` (tableau à 2
dimensions).

Si une valeur est vide, la valeur `null` sera retournée. Dans le cas d'un
attribut multiple, le retour pour une valeur vide (aucune valeur) sera un
tableau vide `array()`.

Pour les attributs de type `array`, la valeur retournée est un tableau à deux
dimensions. La première dimension est l'index (numérique - `0` à `n`) de la
rangée et la deuxième est l'identifiant de l'attribut. Chaque élément contient
une valeur typée.

## Erreurs / Exceptions {#core-ref:1b94ded3-591f-49da-8862-f03f9b8fd47c}

Retourne une exception `Dcp\Exception` dans les cas suivants :

*   L'attribut n'existe pas
*    ou si l'attribut est un attribut ne contenant pas de valeur (type `tab`,
    `frame` ou `menu`).

## Historique {#core-ref:3d3782b1-2fe9-47f3-a241-05b28e926ead}

Aucun.

## Exemples {#core-ref:0e32343d-3eec-43d9-b31b-89fc0516ecca}

Soit la famille suivante :

| BEGIN  |                   | Ma famille        |                  |     | MYFAMILY |         |     |     |      |      |         |                                     |
| ------ | ----------------- | ----------------- | ---------------- | --- | -------- | ------- | --- | --- | ---- | ---- | ------- | ----------------------------------- |
| CLASS  | My\MyFamily       |                   |                  |     |          |         |     |     |      |      |         |                                     |
| DFLDID | auto              |                   |                  |     |          |         |     |     |      |      |         |                                     |
| ICON   | classe.png        |                   |                  |     |          |         |     |     |      |      |         |                                     |
| //     | idattr            | idframe           | label            | T   | A        | type    | ord | vis | need | link | phpfile | phpfunc                             |
|        |                   |                   |                  |     |          |         |     |     |      |      |         |                                     |
| ATTR   | MY_IDENTIFICATION |                   | Identification   | N   | N        | frame   | 10  | W   |      |      |         |                                     |
| ATTR   | MY_NUMBERONE      | MY_IDENTIFICATION | nombre 1         | Y   | N        | int     | 20  | W   |      |      |         |                                     |
| ATTR   | MY_NUMBERTWO      | MY_IDENTIFICATION | nombre 2         | N   | N        | int     | 30  | W   |      |      |         |                                     |
| ATTR   | MY_SUM            | MY_IDENTIFICATION | Nombre 1&plus;2  | N   | N        | int     | 40  | R   |      |      |         | ::mySum(MY_NUMBERONE, MY_NUMBERTWO) |
| ATTR   | MY_REDACTOR       | MY_IDENTIFICATION | Rédacteur        | N   | N        | account | 50  | W   |      |      |         |                                     |
| ATTR   | MY_REF            | MY_IDENTIFICATION | référence        | N   | N        | text    | 70  | W   |      |      |         |                                     |
| ATTR   | MY_T_PARTNER      | MY_IDENTIFICATION | Participants     | N   | N        | array   | 80  | W   |      |      |         |                                     |
| ATTR   | MY_COAUTHORS      | MY_T_PARTNER      | Co-auteurs       | N   | N        | account | 90  | W   |      |      |         |                                     |
| ATTR   | MY_CODATE         | MY_T_PARTNER      | Dates d'entrée   | N   | N        | date    | 100 | W   |      |      |         |                                     |
|        |                   |                   |                  |     |          |         |     |     |      |      |         |                                     |
| END    |                   |                   |                  |     |          |         |     |     |      |      |         |                                     |


Vérification de l'attribut `my_sum` en fonction des valeurs de `my_numberone` et
`my_numbertwo` 

    [php]
    use \Dcp\AttributeIdentifiers\MyFamily as Attributes\MyFamily;
    
    /** @var \Dcp\Family\MyFamily */
    $myDoc = \Dcp\DocManager::getDocument( "MY_DOCUMENT");
    if ($myDoc && $myDoc->isAlive()) {
        $n1=$myDoc->getAttributeValue(Attributes\MyFamily::my_numberone);
        $n2=$myDoc->getAttributeValue(Attributes\MyFamily::my_numbertwo);
        $sum=$myDoc->getAttributeValue(Attributes\MyFamily::my_sum);
        if ($sum != ($n1 + $n2)) {
            printf("La somme est incorrecte : %d + %d <> %d ! \n", $n1, $n2, $sum);
        }
    }

Retour d'un tableau. Le document a deux rangées.

    [php]
    use \Dcp\AttributeIdentifiers\MyFamily as Attributes\MyFamily;
    
    /** @var \Dcp\Family\MyFamily */
    $myDoc = \Dcp\DocManager::getDocument( "MY_DOCUMENT");
    if ($myDoc && $myDoc->isAlive()) {
        $partipants=$myDoc->getAttributeValue(Attributes\MyFamily::my_t_partner);
        print_r($partipants);
    }

    [php]
    Array
    (
        [0] => Array
            (
                [my_coauthors] => "5838"
                [my_codate] => DateTime Object
                    (
                        [date] => "2013-07-04 00:00:00"
                        [timezone_type] => 3
                        [timezone] => Europe/Paris
                    )
            )
        [1] => Array
            (
                [my_coauthors] => "5837"
                [my_codate] => DateTime Object
                    (
                        [date] => "2013-06-11 00:00:00"
                        [timezone_type] => 3
                        [timezone] => "Europe/Paris"
                    )
            )
    )



## Notes {#core-ref:df249de8-f433-430a-8208-6049b367ca85}

Aucunes.

## Voir aussi {#core-ref:ea3b6c2b-df66-4a38-9dfa-4176f05c8c66}

*   [`Doc::setAttributeValue()`][docsetattrvalue],
*   [`Doc::getRawValue()`][docgetrawvalue].

<!-- links -->
[docgetattrvalue]:  #core-ref:e4a8d6ff-7229-4105-81c4-94773ac24dfd
[docgetrawvalue]:   #core-ref:f779391c-ee61-4c3a-8976-6b74f83ecc8f
[docgetoldvalue]:   #core-ref:dccf7c64-8f4f-4c4a-8d0d-79b21b924848
[docsetvalue]:      #core-ref:febc397f-e629-4d47-955d-27cab8f4ed2f
[docsetattrvalue]:  #core-ref:1e766800-b2e1-462b-9793-b0261ede8677
[computeattr]:      #core-ref:4565cab9-73c8-4eee-bfa7-218ffbd4b687
[phpdatetime]:      http://www.php.net/manual/fr/book.datetime.php "DateTime sur le site PHP"
[phpint]:           http://www.php.net/manual/fr/language.types.integer.php "nombre entier sur le site PHP"
[phpdouble]:        http://www.php.net/manual/fr/language.types.float.php "nombres décimaux sur le site PHP"