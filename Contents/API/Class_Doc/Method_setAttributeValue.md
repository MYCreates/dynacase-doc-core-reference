# Doc::setAttributeValue() {#core-ref:1e766800-b2e1-462b-9793-b0261ede8677}

<div class="short-description">
Modifie la valeur d'un attribut de document.
</div>

## Description {#core-ref:d0e5e69d-4f7f-4b77-87f7-818656954e90}

    [php]
    void setAttributeValue ( string $attributeIdentifier,
                              mixed $value )

Modifie la valeur d'un attribut de l'objet _Document_. La modification n'est pas
enregistrée en base de données.

### Avertissements {#core-ref:130a9c3c-80e2-4081-b302-d0b81200efc2}

Pour enregistrer la modification , il est nécessaire d'appeler la méthode
[`Doc::store()`][docstore].

Les résultats fournis par la méthode
[`Doc::getRawValue()`][docgetrawvalue] ne peuvent pas être utilisés
directement par cette méthode.

Les espaces et certains caractères invisibles en début et fin de valeur sont
supprimés. La fonction [`trim`][phptrim] est utilisée pour réaliser cette
suppression.

## Liste des paramètres {#core-ref:4d1ee0a2-6e51-4e69-9e6e-0f55a96374b5}

(string) `attributeIdentifier`
:   Identifiant de l'attribut à modifier.

(mixed) `value` 
:   Nouvelle [valeur **typée**][doctypevalue]. Le type de la valeur est fonction
    du type de l'attribut (voir [les types en fonction de la
    valeur][doctypevalue]).
    
    Il est possible d'affecter aussi les attributs de type `array` avec un
    tableau à 2 dimensions. La première dimension est l'index de la rangée, la
    deuxième dimension est l'identifiant de l'attribut.

## Valeur de retour {#core-ref:1c2cdade-e90b-4144-bda0-bb6abde92936}

Void.

## Erreurs / Exceptions {#core-ref:caba99e2-da92-4a09-803c-f448a1d838a9}

Retourne une exception de type `\Dcp\Exception` dans les cas suivants :

*   L'attribut n'existe pas;
*   Le type attribut ne peut recevoir de valeur (`frame`, `tab`, `menu`)

Retourne une exception de type `\Dcp\AttributeValue\Exception` dans les cas
suivants :

*   La valeur est incompatible avec le type d'attribut;
*   Une valeur scalaire est donnée pour un attribut multiple.

## Historique {#core-ref:2ac0c68c-897b-4ed6-97c9-5d36c8e49db9}

Aucun.

## Exemples {#core-ref:9ebcbb07-2012-4ef0-9e14-978d4a1762ce}

Calcul de la somme des attributs `my_numberone` et `my_numbertwo` et
enregistrement dans l'attribut `my_sum`.

Mise à jour de l'attribut `my_mail` avec l'adresse courriel du rédacteur.

Soit la famille suivante :

| BEGIN |                   | Ma famille        |                  |     | MYFAMILY |         |     |     |   |         |     |
| ----- | ----------------- | ----------------- | ---------------- | --- | -------- | ------- | --- | --- | - | ------- | --- |
| CLASS | My\MyFamily       |                   |                  |     |          |         |     |     |   |         |     |
| //    | idattr            | idframe           | label            | T   | A        | type    | ord | vis | … | phpfunc |     |
| ATTR  | MY_IDENTIFICATION |                   | Identification   | N   | N        | frame   | 10  | W   |   |         |     |
| ATTR  | MY_NUMBERONE      | MY_IDENTIFICATION | nombre 1         | Y   | N        | int     | 20  | W   |   |         |     |
| ATTR  | MY_NUMBERTWO      | MY_IDENTIFICATION | nombre 2         | N   | N        | int     | 30  | W   |   |         |     |
| ATTR  | MY_REDACTOR       | MY_IDENTIFICATION | rédacteur        | N   | N        | account | 30  | W   |   |         |     |
| ATTR  | MY_MAIL           | MY_IDENTIFICATION | Adresse courriel | N   | N        | text    | 10  | R   |   |         |     |
| ATTR  | MY_SUM            | MY_IDENTIFICATION | nombre 1&plus;2  | N   | N        | int     | 30  | R   |   |         |     |
| END   |                   |                   |                  |     |          |         |     |     |   |         |     |


### Mise à jour d'attribut après modification du document {#core-ref:83e79e19-4487-4f42-8aab-0839b2d4a022}

Avec la classe :

    [php]
    namespace My;
    use \Dcp\AttributeIdentifiers\MyFamily as MyAttributes;
    use \Dcp\AttributeIdentifiers\Iuser as Aiuser;
    
    class MyFamily extends \Dcp\Family\Document
    {
        /**
         * Mise à jour de l'attribut `my_sum` 
         */
        protected function setMySum()
        {
            $n1 = $this->getAttributeValue(MyAttributes::my_numberone);
            $n2 = $this->getAttributeValue(MyAttributes::my_numbertwo);
            $this->setAttributeValue(MyAttributes::my_sum, ($n1 + $n2));
        }
        /**
         * Mise à jour de l'attribut `my_mail` avec l'adresse courriel du rédacteur
         */
        protected function setMyMail()
        {
            $redacId = $this->getAttributeValue(MyAttributes::my_redactor);
            if ($redacId === null) {
                $this->clearValue(MyAttributes::my_mail);
            } else {
                $redacDoc=\Dcp\DocManager::getDocument($redacId );
                if ($redacDoc && $redacDoc->isAlive()) {
                    $this->setAttributeValue(MyAttributes::my_mail, $redacDoc->getAttributeValue(AIuser::us_mail));
                } else {
                    $this->clearValue(MyAttributes::my_mail);
                }
            }
        }
        public function postStore()
        {
            $err = parent::postStore();
            if (empty($err)) {
                    $this->setMySum();
                    $this->setMyMail();
            }
            return $err;
        }
    }

### Exemple avec traitement des exceptions {#core-ref:94b572da-55cf-49c5-8d64-a1d64e4f9412}

Calcul de la prochaine échéance dans 7 jours après la dernière modification.

Avec la classe :

    [php]
    namespace My;
    use \Dcp\AttributeIdentifiers\MyFamily as MyAttributes;
    use \Dcp\AttributeIdentifiers\Iuser as Aiuser;
    
    class MyFamily extends \Dcp\Family\Document
    {
        /**
         * Prochaine échéance dans 7 jours
         */
        protected function setNextDate()
        {
            $d = new \DateTime('now');
            $d->modify('+7 day');
            $this->setAttributeValue(MyAttributes::my_nextdate, $d);
        }
        public function postStore()
        {
            $err = parent::postStore();
            if (empty($err)) {
                try {
                    $this->setNextDate();
                } catch (\Dcp\AttributeValue\Exception $e) {
                    $err = sprintf("Valeur erronée %s", $e->getDcpMessage());
                } catch (\Dcp\Exception $e) {
                    $err = sprintf("Erreur d'attribut %s", $e->getDcpMessage());
                }
            }
            return $err;
        }
    }

### Exemple d'affectation de tableau {#core-ref:65d743bd-c944-4841-b9f3-a77ff9dc25ac}
Soit les familles suivantes :

| BEGIN  |                   |     Ma famille    |                |     | MYFAMILY |         |     |     |
| ------ | ----------------- | ----------------- | -------------- | --- | -------- | ------- | --- | --- |
| CLASS  | My\MyFamily       |                   |                |     |          |         |     |     |
| DFLDID | auto              |                   |                |     |          |         |     |     |
| ICON   | classe.png        |                   |                |     |          |         |     |     |
| //     | idattr            | idframe           | label          | T   | A        | type    | ord | vis |
| ATTR   | MY_IDENTIFICATION |                   | Identification | N   | N        | frame   | 10  | W   |
| ATTR   | MY_REDACTOR       | MY_IDENTIFICATION | Rédacteur      | N   | N        | account | 50  | W   |
| ATTR   | MY_MAIL           | MY_IDENTIFICATION | Mail           | N   | N        | text    | 60  | R   |
| ATTR   | MY_REF            | MY_IDENTIFICATION | référence      | N   | N        | text    | 70  | W   |
| ATTR   | MY_T_PARTNER      | MY_IDENTIFICATION | Participants   | N   | N        | array   | 80  | W   |
| ATTR   | MY_COAUTHORS      | MY_T_PARTNER      | Co-auteurs     | N   | N        | account | 90  | W   |
| ATTR   | MY_COMAIL         | MY_T_PARTNER      | Adresse mail   | N   | N        | text    | 110 | W   |
|        |                   |                   |                |     |          |         |     |     |
| END    |                   |                   |                |     |          |         |     |     |

| BEGIN |              | Contact     |              |     | MYCONTACT |       |     |     |      |                  |   |                                             |     |            |
| ----- | ------------ | ----------- | ------------ | --- | --------- | ----- | --- | --- | ---- | ---------------- | - | ------------------------------------------- | --- | ---------- |
| //    | idattr       | idframe     | label        | T   | A         | type  | ord | vis | need | link             | … | phpfunc                                     | ... | option     |
| ATTR  | MY_FR_IDENT  |             | État civil   | N   | N         | frame |     | W   |      |                  |   |                                             |     |            |
| ATTR  | MY_NAME      | MY_FR_IDENT | nom          | Y   | N         | text  | 30  | W   | Y    |                  |   |                                             |     |            |
| ATTR  | MY_FNAME     | MY_FR_IDENT | prénom       | Y   | N         | text  | 35  | W   | Y    |                  |   |                                             |     |            |
| ATTR  | MY_MAIL      | MY_FR_IDENT | mail         | N   | Y         | text  | 40  | W   |      | mailto:%MY_MAIL% |   |                                             |     |            |
| ATTR  | MY_ISPARTNER | MY_FR_IDENT | Partenaire ? | N   | Y         | enum  | 45  | W   |      |                  |   | no&#124;Pas partenaire, yes&#124;Partenaire |     | system=yes |
| END   |              |             |              |     |           |       |     |     |      |                  |   |                                             |     |            |


La méthode `setMyPartners` va rechercher tous les contacts *partenaire* et va
les insérer dans le tableau `my_t_partner` en renseignant l'identifiant et
l'adresse mail de chaque partenaire.

    [php]
    namespace My;
    use \Dcp\AttributeIdentifiers\MyFamily as MyAttributes;
    use \Dcp\AttributeIdentifiers\MyContact as AMyContact;
    
    class MyFamily extends \Dcp\Family\Document
    {
        protected function setMyPartners()
        {
            // Recherche des partenaires
            $s = new \SearchDoc($this->dbaccess, \Dcp\Family\Mycontact::familyName);
            $s->setObjectReturn(true);
            $s->addFilter("%s = 'yes'", AMyContact::my_ispartner);
            $dl = $s->search()->getDocumentList();
            
            // On affecte le tableau à 2 dimensions de valeurs
            $partners=array();
            /** @var \Dcp\Family\MyContact $contact */
            foreach ($dl as $contact) {
                $partners[] = array(
                    MyAttributes::my_coauthors => $contact->id,
                    MyAttributes::my_comail => $contact->getAttributeValue(AMyContact::my_mail));
            }
            $this->setAttributeValue(MyAttributes::my_t_partner, $partners);
        }
        
        public function postStore()
        {
            $this->setMyPartners();
        }
    }


## Notes {#core-ref:e3096440-920c-4519-9202-111a534ee171}

Pour les attributs de type "numérique" et les types "date", plusieurs types de
valeurs sont autorisés.

|    type   |              valeur              |                       autorisé                      |
| --------- | -------------------------------- | --------------------------------------------------- |
| int       | (int) 3                          | <span class="ok">OK</span>                          |
| int       | (string)"3"                      | <span class="ok">OK</span>                          |
| int       | (float) 3.0                      | <span class="ko">KO</span>                          |
| int       | (string)"a"                      | <span class="ko">KO</span>                          |
| double    | (float) 3.0                      | <span class="ok">OK</span>                          |
| double    | (int) 3                          | <span class="ok">OK</span>                          |
| double    | (string) "3.0"                   | <span class="ok">OK</span>                          |
| double    | (string)"a"                      | <span class="ko">KO</span>                          |
| date      | (DateTime) "2012-03-16"          | <span class="ok">OK</span>                          |
| date      | (string) "2012-03-16"            | <span class="ok">OK</span>                          |
| date      | (string) "2012-03"               | <span class="ko">KO</span>                          |
| date      | (string) "16/03/2012"            | <span class="ok">OK</span> (dépendant de la locale) |
| timestamp | (DateTime) "2012-03-16"          | <span class="ok">OK</span>                          |
| timestamp | (DateTime) "2012-03-16 12:54:34" | <span class="ok">OK</span>                          |
| timestamp | (string) "2012-03-16 12:54:34"   | <span class="ok">OK</span>                          |
| timestamp | (string) "2012-03-16T12:54:34"   | <span class="ok">OK</span>                          |


## Voir aussi {#core-ref:42b9caf9-a56b-4880-8def-2c3ebd0e20c4}

*   [`Doc::setValue()`][docsetvalue],
*   [`Doc::getAttributeValue()`][docgetattrvalue],
*   [`Doc::postStore()`][docpoststore].

<!-- links -->
[docstore]:         #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[docgetattrvalue]:  #core-ref:e4a8d6ff-7229-4105-81c4-94773ac24dfd
[docgetrawvalue]:   #core-ref:f779391c-ee61-4c3a-8976-6b74f83ecc8f
[docgetoldvalue]:   #core-ref:dccf7c64-8f4f-4c4a-8d0d-79b21b924848
[docsetvalue]:      #core-ref:febc397f-e629-4d47-955d-27cab8f4ed2f
[docgetrawvalue]:   #core-ref:f779391c-ee61-4c3a-8976-6b74f83ecc8f
[doctypevalue]:     #core-ref:b2e45d57-63f8-408f-9779-ebf35e6967e4
[docpoststore]:     #core-ref:99520a31-0aef-4bc6-b20a-114737059d17 "Hameçon Doc::postStore()"
[phptrim]:          http://www.php.net/manual/fr/function.trim.php "fonction trim sur le site php.net"