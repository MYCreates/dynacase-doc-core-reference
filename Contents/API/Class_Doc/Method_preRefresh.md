# Doc::preRefresh() {#core-ref:580d6be1-6b6a-439b-abd7-34b26cfaf2e5}

<div class="short-description" markdown="1">
[Hameçon][hook] (ou hook) utilisé par la méthode [`Doc::refresh()`][docrefresh].
</div>

## Description {#core-ref:ae076285-e189-4974-a637-14296192545f}

    [php]
    string preRefresh ()

Cette méthode est appelée par la méthode [`Doc::refresh()`][docrefresh] 
**avant** la mise à jour des [attributs calculés][computeattr]. 

### Avertissements {#core-ref:605f2425-8887-4085-add7-5d91809198ae}

Les contrôles relatifs au droit de modification sont désactivés pendant l'appel
de cette méthode. Ceci permet à la méthode [`Doc::setValue()`][docsetvalue] de
ne pas vérifier les droits de modification.

## Liste des paramètres {#core-ref:07455377-c9a5-4d8f-8fef-d4d7b90362e1}

Aucun.

## Valeur de retour {#core-ref:e3f80887-87c0-464f-8f75-12c429c36225}

Cette méthode doit retourner une chaîne de caractères. Ce message est retourné
par la méthode [`Doc::refresh()`][docrefresh].

## Erreurs / Exceptions {#core-ref:6ea9af28-baa7-458f-b2f3-fff17d51ff4d}

Aucun.

## Historique {#core-ref:5d97339d-ed28-4a7d-ac62-ef93ce8506a6}

Aucun.

## Exemples {#core-ref:057d6330-a8b0-466c-b953-7d8beefbd8d3}

Récupération de l'adresse mail du rédacteur et mise à jour. Si l'adresse est
différente un message est retourné.

Soit la famille suivante :

| BEGIN |                   | Ma famille        |                  |     | MYFAMILY |         |     |     |   |                                     |     |
| ----- | ----------------- | ----------------- | ---------------- | --- | -------- | ------- | --- | --- | - | ----------------------------------- | --- |
| CLASS | My\MyFamily       |                   |                  |     |          |         |     |     |   |                                     |     |
| //    | idattr            | idframe           | label            | T   | A        | type    | ord | vis | … | phpfunc                             |     |
| ATTR  | MY_IDENTIFICATION |                   | Identification   | N   | N        | frame   | 10  | W   |   |                                     |     |
| ATTR  | MY_REDACTOR       | MY_IDENTIFICATION | rédacteur        | N   | N        | account | 30  | W   |   | ::mySum(MY_NUMBERONE, MY_NUMBERTWO) |     |
| ATTR  | MY_MAIL           | MY_IDENTIFICATION | Adresse courriel | N   | N        | text    | 10  | R   |   |                                     |     |
| END   |                   |                   |                  |     |          |         |     |     |   |                                     |     |

Avec la classe :

    [php]
    namespace My;
    use \Dcp\AttributeIdentifiers\MyFamily as MyAttributes;
    use \Dcp\AttributeIdentifiers\Iuser as Aiuser;
    
    class MyFamily extends \Dcp\Family\Document
    {
        public function preRefresh()
        {
            $msg = '';
            $redactorId = $this->getRawValue(MyAttributes::my_redactor);
            if ($redactorId) {
                $du = \Dcp\DocManager::getDocument($redactorId);
                if ($du && $du->isAlive()) {
                    $redactorMail=$du->getRawValue(Aiuser::us_mail);
                    if ($redactorMail != $this->getRawValue(MyAttributes::my_mail)) {
                        $err=$this->setValue(MyAttributes::my_mail, $du->getRawValue(Aiuser::us_mail));
                        if (empty($err)) {
                            $msg=sprintf("L'adresse du rédacteur a été changée.\nNouvelle adresse : %s", $redactorMail);
                        } else {
                            $msg=sprintf("Erreur d'affectation : %s", $err);
                        }
                    }
                }
            } else {
                $this->clearValue(MyAttributes::my_mail);
            }
            return $msg;
        }
    }


## Notes {#core-ref:f41b94f9-2887-4549-9768-7ae711e18bdc}

En cas de famille héritée, il est nécessaire d'appeler l'hameçon du parent pour
disposer des mêmes fonctionnalités.

## Voir aussi {#core-ref:a46fb72c-91f0-4cd0-b08e-264c8fd1be8e}


*   [`Doc::refresh()`][docrefresh],
*   [`Doc::postRefresh()`][docpostrefresh].

<!-- links -->
[docprerefresh]:    #core-ref:580d6be1-6b6a-439b-abd7-34b26cfaf2e5 "Hameçon Doc::preRefresh()"
[docpostrefresh]:   #core-ref:9352c534-3691-41e3-b293-599db8e9a4fd "Hameçon Doc::postRefresh()"
[docrefresh]:       #core-ref:0bab02de-50e0-46f2-8bdb-81c62dc86c93
[computeattr]:      #core-ref:4565cab9-73c8-4eee-bfa7-218ffbd4b687
[docsetvalue]:      #core-ref:febc397f-e629-4d47-955d-27cab8f4ed2f
[hook]:             https://fr.wikipedia.org/wiki/Hook_(informatique)