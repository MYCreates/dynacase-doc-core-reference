# Doc::getCustomSearchValues()  {#core-ref:f1aaac21-085c-4ef6-bddc-962530c4efaa}

<div class="short-description" markdown="1">
[Hameçon][hooks] (ou hook) utilisé par la méthode [`Doc::Store()`][docstore].

Cette méthode permet d'ajouter des informations supplémentaires pour la
[recherche globale][globalsearch].

</div>

Les valeurs stockées dans les [colonnes `svalues` et `fulltext`][dbdoc] sont les
données brutes des attributs recherchables (option
[searchcriteria][searchcriteria]). 

<span class="flag from release inline">3.2.21</span>Les valeurs affichées des
attributs sont aussi enregistrées. Pour une relation l'identifiant sera stocké
ainsi que son [titre][getcustomtitle], pour la date le format iso sera
enregistré ainsi que le format dans les [locales][locales] définies, le nom du
jour et le nom du mois dans les différentes traductions disponibles et les
nombres composant la date. Pour les énumérés la clef ainsi que les [traductions
des labels][i18nfam] sont enregistrés.

## Description  {#core-ref:ef0c1f0d-986c-4768-8ddb-a90d0b145929}

    [php]
    protected function string[] getCustomSearchValues ()

Cette méthode doit retourner un tableau de chaîne de caractères contenant
des mots supplémentaires. Ces mots servent à retrouver le document en utilisant
la [recherche globale][globalsearch].


### Avertissements  {#core-ref:44e7970f-45a3-46f4-b59c-abb0b0f9e4af}

Ce hook n'est pas déclenché pour les documents temporaire. Ce hook est déclenché
par la méthode [`store`][docstore] en cas de création. En cas de modification,
il est déclenché uniquement si une valeur d'attribut a changé.

## Liste des paramètres {#core-ref:af2917ea-b283-414d-97f7-f6236ca4da3b}

Aucun.

## Valeur de retour  {#core-ref:26fb7c9e-97ed-48e2-9393-c11e223d4f74}

Un tableau contenant les chaînes contenant les mots à ajouter.

## Erreurs / Exceptions  {#core-ref:31a580de-6433-4b7e-bbe9-8b5d16e5d56e}

Aucune.

## Historique  {#core-ref:bd21379e-bd98-4284-841f-d61ca9f886ab}

Aucun.

## Exemples {#core-ref:acf8bfa3-53b2-41da-a34c-458943bbc439}

Dans la famille _MyFamily_, Un administrateur modifie le code du jour chaque
jour. Ce message doit être enregistré dans les valeurs recherchables afin de
réaliser des recherches par code. Le code étant enregistrés à chaque
modification.

Soit la famille suivante :

| BEGIN |                   |     Ma famille    |                |     | MYFAMILY |                   |     |     |
| ----- | ----------------- | ----------------- | -------------- | --- | -------- | ----------------- | --- | --- |
| CLASS | My\MyFamily       |                   |                |     |          |                   |     |     |
| //    | idattr            | idframe           | label          | T   | A        | type              | ord | vis |
| ATTR  | MY_IDENTIFICATION |                   | Identification | N   | N        | frame             | 10  | W   |
| ATTR  | MY_RELATION       | MY_IDENTIFICATION | Relation       | Y   | N        | docid("MYFAMILY") | 20  | W   |
| ATTR  | MY_DATE           | MY_IDENTIFICATION | Date           | N   | N        | date              | 30  | W   |
| PARAM | MY_PARAMETERS     |                   | Paramètres     | N   | N        | frame             | 10  | W   |
| PARAM | MY_CODE           | MY_PARAMETERS     | Code du jour   | N   | N        | text              | 20  | W   |
| END   |                   |                   |                |     |          |                   |     |     |


    [php]
    namespace My;
    use \Dcp\AttributeIdentifiers\MyFamily as MyAttributes;
    
    class MyFamily extends \Dcp\Family\Document
    {
        protected function getCustomSearchValues()
        {
            $customValues=parent::customSearchValue();
            $customValues[]=$this->getFamilyParameterValue(MyAttributes::my_code);
            return $customValues;
        }
    }


Si my_relation="1234" (titre="Mon document lié"), my_date="2016-04-21", my_code="ATY67"

Alors les valeurs recherchables contiendront :

*   1234
*   Mon document lié
*   2016-04-21
*   21/04/2016
*   21
*   04
*   2016
*   04/21/2016
*   avril
*   april
*   jeudi
*   thursday
*   ATY67


## Notes  {#core-ref:e915e195-c24c-4fd4-9f1e-2394cd52c2fa}

En cas de famille héritée, il est nécessaire d'appeler l'hameçon du parent pour
conserver les éventuelles valeurs ajoutées par les familles mères..

## Voir aussi  {#core-ref:035a445a-7e96-43ba-9768-24a48a586299}

*   [`Doc::store()`][docstore].

<!-- links -->

[docstore]:         #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[docpostcreated]:   #core-ref:b8f80e6b-a374-4bf4-bc76-47290cd69c45 "Hameçon Doc::postCreated()"
[docpoststore]:     #core-ref:99520a31-0aef-4bc6-b20a-114737059d17 "Hameçon Doc::postStore()"
[docprestore]:      #core-ref:3517da95-82fe-4adb-8bc4-ef49ca55edb0 "Hameçon Doc::preStore()"
[docprecreated]:    #core-ref:e85aa9d4-5e62-4a60-9d1c-f60433301747 "Hameçon Doc::preCreated()"
[docprerefresh]:    #core-ref:580d6be1-6b6a-439b-abd7-34b26cfaf2e5 "Hameçon Doc::preRefresh()"
[docpostrefresh]:   #core-ref:9352c534-3691-41e3-b293-599db8e9a4fd "Hameçon Doc::postRefresh()"
[docrevise]:        #core-ref:882e3730-0483-4dbc-9b9d-0d0b5cc31d38
[hook]:             https://fr.wikipedia.org/wiki/Hook_(informatique)
[dbdoc]:            #core-ref:d5fc4bc8-067d-4dd8-b617-9641e3b11707
[searchcriteria]:   #core-ref:16e19c90-3233-11e2-a58f-6b135c3a2496
[globalsearch]:     #core-ref:19b9f4b4-c960-46eb-b4e0-805ed76be3a6
[getcustomtitle]:   #core-ref:3c5ff78d-c080-48fb-a293-9736ed4e95b8
[i18nfam]:          #core-ref:f5872ef4-4170-11e3-ba58-48f953959281
[locales]:          #core-ref:2b79d76a-fe0d-446e-bee4-c73e36f64a41
[hooks]:            #core-ref:8f3d47de-32b5-4748-8a00-b1569c5423e5