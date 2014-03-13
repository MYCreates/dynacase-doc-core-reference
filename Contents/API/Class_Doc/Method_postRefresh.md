# Doc::postRefresh() {#core-ref:9352c534-3691-41e3-b293-599db8e9a4fd}
<div class="short-description" markdown="1">
[Hameçon][hook] (ou hook) utilisé par la méthode [`Doc::refresh()`][docrefresh].
</div>

## Description {#core-ref:1c504ccc-70d8-4e3b-a725-a31c1d72db90}

    [php]
    string postRefresh ()

Cette méthode est appelée par la méthode [`Doc::refresh()`][docrefresh] 
**après** la mise à jour des [attributs calculés][computeattr]. 

### Avertissements {#core-ref:e947ed11-d964-4609-8ed2-72b038ef60af}

Les contrôles relatifs au droit de modification sont désactivés pendant l'appel
de cette méthode. Ceci permet à la méthode [`Doc::setValue()`][docsetvalue] de
ne pas vérifier les droits de modification.

## Liste des paramètres {#core-ref:1006ecfc-2d97-4a2c-ac12-4e8be5f7e40d}

Aucun.

## Valeur de retour {#core-ref:4d5ffcb0-f7bf-4dd1-8c5e-00019f2a31f9}

Cette méthode doit retourner un message. Ce message est retourné par la méthode
[`Doc::refresh()`][docrefresh].

## Erreurs / Exceptions {#core-ref:3ebba583-f0dd-4c88-9a3c-0e2e52aaef2b}

Aucun.

## Historique {#core-ref:be53eb63-b6a2-4168-9121-909f22bce989}

Cette méthode était anciennement nommée `specRefresh()`.

## Exemples {#core-ref:e61b454f-4c98-40e1-b601-8062ec6a1c38}

Affichage d'un message si l'attribut calculé `my_sum` est supérieur au paramètre
`my_max`. Dans ce cas l'attribut `my_sum` est mis à jour juste avant l'appel à
l'hameçon `postRefresh()`.

Soit la famille suivante :

| BEGIN |                   | Ma famille        |                 |     | MYFAMILY |       |     |     |   |                                     |     |
| ----- | ----------------- | ----------------- | --------------- | --- | -------- | ----- | --- | --- | - | ----------------------------------- | --- |
| CLASS | My\MyFamily       |                   |                 |     |          |       |     |     |   |                                     |     |
| //    | idattr            | idframe           | label           | T   | A        | type  | ord | vis | … | phpfunc                             |     |
| ATTR  | MY_IDENTIFICATION |                   | Identification  | N   | N        | frame | 10  | W   |   |                                     |     |
| ATTR  | MY_NUMBERONE      | MY_IDENTIFICATION | nombre 1        | Y   | N        | int   | 20  | W   |   |                                     |     |
| ATTR  | MY_NUMBERTWO      | MY_IDENTIFICATION | nombre 2        | N   | N        | int   | 30  | W   |   |                                     |     |
| ATTR  | MY_SUM            | MY_IDENTIFICATION | nombre 1&plus;2 | N   | N        | int   | 30  | R   |   | ::mySum(MY_NUMBERONE, MY_NUMBERTWO) |     |
| PARAM | MY_PARAMETERS     |                   | Paramètres      | N   | N        | frame | 10  | W   |   |                                     |     |
| PARAM | MY_MAX            | MY_PARAMETERS     | max             | N   | N        | int   | 20  | W   |   |                                     |     |
| END   |                   |                   |                 |     |          |       |     |     |   |                                     |     |

Avec la classe :

    [php]
    namespace My;
    use \Dcp\AttributeIdentifiers\MyFamily as MyAttributes;
    
    class MyFamily extends \Dcp\Family\Document
    {
        public function mySum($x, $y)
        {
            return ($x + $y);
        }
        public function postRefresh()
        {
            $msg = '';
            $sum = $this->getAttributeValue(MyAttributes::my_sum);
            $max = $this->getFamilyParameterValue(MyAttributes::my_max);
            if ($sum > $max) {
                $msg = sprintf("Max %d is reached", $max);
            }
            return $msg;
        }
    }

## Notes {#core-ref:47c8f567-875a-48a5-943d-7af334150497}

En cas de famille héritée, il est nécessaire d'appeler l'hameçon du parent pour
disposer des mêmes fonctionnalités.

## Voir aussi {#core-ref:01409847-c705-426c-92f2-e68726f948ba}

*   [`Doc::refresh()`][docrefresh],
*   [`Doc::preRefresh()`][docprerefresh].

<!-- links -->
[docprerefresh]:    #core-ref:580d6be1-6b6a-439b-abd7-34b26cfaf2e5 "Hameçon Doc::preRefresh()"
[docpostrefresh]:   #core-ref:9352c534-3691-41e3-b293-599db8e9a4fd "Hameçon Doc::postRefresh()"
[docrefresh]:       #core-ref:0bab02de-50e0-46f2-8bdb-81c62dc86c93
[computeattr]:      #core-ref:4565cab9-73c8-4eee-bfa7-218ffbd4b687
[hook]:             https://fr.wikipedia.org/wiki/Hook_(informatique)
[docsetvalue]:      #core-ref:febc397f-e629-4d47-955d-27cab8f4ed2f