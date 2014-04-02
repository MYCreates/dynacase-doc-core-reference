# Doc::getOldRawValues() {#core-ref:d76b0955-26c8-49c8-9e34-82c41e0e4def}

<div class="short-description">

Récupère la liste des valeurs des attributs modifiés.

</div>

## Description {#core-ref:fa85ee0d-6980-401c-ba36-3aab8e147661}

    [php]
    string[] getOldRawValues (  )

Récupère la liste des valeurs des attributs modifié depuis l'instanciation de
l'objet Document. Voir [Doc::getOldRawValue()][docgetOldValue] pour plus de
détails.

### Avertissements {#core-ref:23cffd84-bf77-4636-8c41-5762890f54da}

Aucun.

## Liste des paramètres {#core-ref:3ceaec53-3e46-41d1-8034-a3506219dc61}

Aucun.

## Valeur de retour {#core-ref:50244fd3-0a0c-449f-8b77-9b741f85696a}

Retourne un tableau indexé par l'identifiant de l'attribut (en minuscules).
Chaque attribut modifié a sa propre entrée dans le tableau. Les attributs non
modifiés n'ont pas d'entrée dans le tableau.

## Erreurs / Exceptions {#core-ref:9dc9b65f-32fb-4997-8ff1-1817e4ecccb6}

Aucune.

## Historique {#core-ref:b4f058a2-809c-473d-aa8e-d779d818cc7c}

Cette méthode était anciennement nommée `getOldValues`.

## Exemples {#core-ref:ff6026de-8afb-4bcb-9680-b32edf234007}

Soit la famille suivante :

| BEGIN |                   |     Ma famille    |                 |     | MYFAMILY |         |     |     |     |                                     |     |
| ----- | ----------------- | ----------------- | --------------- | --- | -------- | ------- | --- | --- | --- | ----------------------------------- | --- |
| CLASS | My\MyFamily       |                   |                 |     |          |         |     |     |     |                                     |     |
| //    | idattr            | idframe           | label           | T   | A        | type    | ord | vis | ... | phpfunc                             |     |
| ATTR  | MY_IDENTIFICATION |                   | Identification  | N   | N        | frame   | 10  | W   |     |                                     |     |
| ATTR  | MY_REDACTOR       | MY_IDENTIFICATION | rédacteur       | N   | N        | account | 30  | W   |     |                                     |     |
| ATTR  | MY_NUMBERONE      | MY_IDENTIFICATION | nombre 1        | Y   | N        | int     | 20  | W   |     |                                     |     |
| ATTR  | MY_NUMBERTWO      | MY_IDENTIFICATION | nombre 2        | N   | N        | int     | 30  | W   |     |                                     |     |
| ATTR  | MY_SUM            | MY_IDENTIFICATION | nombre 1&plus;2 | N   | N        | int     | 30  | R   |     | ::mySum(MY_NUMBERONE, MY_NUMBERTWO) |     |
| END   |                   |                   |                 |     |          |         |     |     |     |                                     |     |

Avec la classe :

    [php]
    namespace My;
    use \Dcp\AttributeIdentifiers\MyFamily as MyAttributes;
    
    class MyFamily extends \Dcp\Family\Document
    {
        public function mySum($x, $y)
        {
            return ($x + $y);
        }
    }

Le code suivant indique le résultat de `getOldRawvalues()`.

    [php]
    if ($myDoc && $myDoc->isAlive()) {
        print "### Valeurs initiales ### \n";
        print_r($myDoc->getValues());
        $myDoc->setValue(Attributes\MyFamily::my_numberone, 342);
        $myDoc->setValue(Attributes\MyFamily::my_numbertwo, 352);
        $myDoc->refresh();
        
        print "### Nouvelles valeurs  ### \n";
        print_r($myDoc->getValues());
        
        print "### Anciennes valeurs modifiées  ### \n";
        print_r($myDoc->getOldRawValues());
    }

Résultat :

    [php]
    ### Valeurs initiales ### 
    Array
    (
        [my_numberone] => 34
        [my_numbertwo] => 35
        [my_sum] => 69
        [my_redactor] => 5838
    )
    
    ### Nouvelles valeurs  ### 
    Array
    (
        [my_numberone] => 342
        [my_numbertwo] => 352
        [my_sum] => 694
        [my_redactor] => 5838
    )
    ### Anciennes valeurs modifiées  ### 
    Array
    (
        [my_numberone] => 34
        [my_numbertwo] => 35
        [my_sum] => 69
    )

## Notes {#core-ref:9a772cad-1ad6-4466-844e-4626380c0a9d}

Aucunes.

## Voir aussi {#core-ref:3cc4e325-c9e0-446c-9daa-919ae542f4c0}


*   [`Doc::getOldRawValue()`][docgetOldValue],
*   [`Doc::store()`][docstore],
*   [`Doc::preCreated()`][docprecreated],
*   [`Doc::preStore()`][docprestore],
*   [`Doc::postCreated()`][docpostcreated].

<!-- links -->
[docstore]:         #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[docpostcreated]:   #core-ref:b8f80e6b-a374-4bf4-bc76-47290cd69c45 "Hameçon Doc::postCreated()"
[docpoststore]:     #core-ref:99520a31-0aef-4bc6-b20a-114737059d17 "Hameçon Doc::postStore()"
[docprestore]:      #core-ref:3517da95-82fe-4adb-8bc4-ef49ca55edb0 "Hameçon Doc::preStore()"
[docprecreated]:    #core-ref:e85aa9d4-5e62-4a60-9d1c-f60433301747 "Hameçon Doc::preCreated()"
[docprerefresh]:    #core-ref:580d6be1-6b6a-439b-abd7-34b26cfaf2e5 "Hameçon Doc::preRefresh()"
[docpostrefresh]:   #core-ref:9352c534-3691-41e3-b293-599db8e9a4fd "Hameçon Doc::postRefresh()"
[docrevise]:        #core-ref:882e3730-0483-4dbc-9b9d-0d0b5cc31d38
[docsetattrvalue]:  #core-ref:1e766800-b2e1-462b-9793-b0261ede8677
[docclearValue]:    #core-ref:30b0592f-f0cd-498f-bc5f-301891c297e0
[docsetvalue]:      #core-ref:febc397f-e629-4d47-955d-27cab8f4ed2f
[docaddarrayrow]:   #core-ref:d75110cb-24f6-4810-9b62-45ce6fd99e70
[rawValueToArray]:  #core-ref:5f4c4ac5-7429-4d07-94d0-245fe63cbfe5
[docgetOldValue]:   #core-ref:dccf7c64-8f4f-4c4a-8d0d-79b21b924848
