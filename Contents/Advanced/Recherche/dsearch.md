# Recherche détaillée {#core-ref:b0bc6976-8eb3-4ba0-9999-241ecf7b1682}

## Enregistrement d'une recherche détaillée {#core-ref:003fbb11-fd6f-48be-b0cc-4f248a62d3fe}

### Création d'une recherche par famille {#core-ref:7c36fc88-d5bc-4e0c-8cbf-092916168575}

La recherche détaillée (famille `DSEARCH`) a pour but de rechercher des
documents d'une famille donnée. Elle permet d'être utilisée depuis l'interface
graphique ou depuis la classe `SearchDoc` pour avoir une liste de document
suivant des critères pré- établis.

    [php]
    /**
     * @var \Dcp\Family\Dsearch $rd
     */
    $rd= \Dcp\DocManager::createDocument( \Dcp\Family\Dsearch::familyName);
    $rd->setValue(\Dcp\AttributeIdentifiers\Dsearch::se_famid, \Dcp\Family\Iuser::familyName);
    $rd->store();
    
    // Récupération du contenu
    $content=$rd->getContent();
    foreach ($content as $rawDocument) {
        printf("Document %s (%d)\n", $rawDocument["title"], $rawDocument["id"]);
    }

De façon similaire, la classe `SearchDoc` peut utiliser une recherche détaillée.

    [php]
    /**
     * @var \Dcp\Family\Dsearch $rd
     */
    $rd= \Dcp\DocManager::createDocument(\Dcp\Family\Dsearch::familyName);
    $rd->setValue(\Dcp\AttributeIdentifiers\Dsearch::se_famid, \Dcp\Family\Iuser::familyName);
    $rd->store();
    
    $s=new SearchDoc();
    $s->useCollection($rd->initid); // utilisation de la recherche stockée
    $s->setObjectReturn(true);
    $dl=$s->search()->getDocumentList();
    
    // Récupération du contenu
    foreach ($dl as $docid => $doc) {
        printf("Document %s (%d)\n", $doc->getTitle(), $docid);
    }

L'affectation des critères est effectuée en valorisant les attributs du tableau
`se_t_detail` de la famille `DSEARCH`.

    [php]
    use \Dcp\AttributeIdentifiers as Attribute;
    /**
     * @var \Dcp\Family\Dsearch $rd
     */
    $rd= \Dcp\DocManager::createDocument(\Dcp\Family\Dsearch::familyName);
    $rd->setAttributeValue(Attribute\Dsearch::se_famid, \Dcp\Family\Iuser::familyName);
    $rd->setAttributeValue(Attribute\Dsearch::ba_title, "Search Active Users with email");
    $criteria=array(
        array(Attribute\Dsearch::se_attrids => Attribute\Iuser::us_status,
              Attribute\Dsearch::se_funcs   => "=",
              Attribute\Dsearch::se_keys    =>"A"),
        array(Attribute\Dsearch::se_attrids => Attribute\Iuser::us_extmail,
              Attribute\Dsearch::se_funcs   => "is not null",
              Attribute\Dsearch::se_keys    => "")
    );
    
    $rd->setAttributeValue(Attribute\Dsearch::se_t_detail, $criteria);
    $rd->store();

### Enregistrement d'une recherche détaillée avec SearchDoc {#core-ref:09de5965-3a76-497a-89b0-0c95de1b28e4}

La recherche détaillée peut aussi être enregistrée avec une requête arbitraire.
La méthode `DocSearch::addStaticQuery()` permet de modifier la requête issue des
critères du document _recherche_.

    [php]
    $s=new SearchDoc("", \Dcp\Family\Iuser::familyName);
    $s->addFilter('us_extmail is not null');
    $sql=$s->getOriginalQuery();
    
    /**
     * @var \Dcp\Family\Dsearch $rd
     */
    $rd= \Dcp\DocManager::createDocument(\Dcp\Family\Dsearch::familyName);
    $rd->setValue(\Dcp\AttributeIdentifiers\Dsearch::ba_title, "my Search");
    $rd->store();
    // on remplace la requête qui sera utilisée par celle produite par SearchDoc
    $rd->addStaticQuery($sql);

Dans ce cas, le document produit s'il est modifié par l'interface perd ses
caractéristiques spécifiques au dépend des nouveaux critères présents sur
l'interface graphique.

La méthode `DocSearch::addStaticQuery()` ne peut être utilisée que si le
document est déjà enregistré en base de données.

La requête _sql_ enregistrée ne doit pas tenir compte des droits. Ces critères
de droits sont ajoutés lors de l'utilisation de la recherche détaillée.

## Utilisation de méthodes dans l'interface de recherche détaillée  {#core-ref:b318bafc-adf9-4ebc-802d-0cb1a8f82054}

Lors de la création d'une recherche détaillée, il est possible de choisir pour
un critère une méthode comme valeur.

    Exemple : ma_date > ::getDate(-7)

Cet exemple indique que le critère sera la date courante - 7 jours. La méthode
doc::getDate() est une méthode statique de la classe doc.

Les méthodes utilisables dans les recherches détaillées sont les méthodes de la
classe de la famille sur laquelle porte la recherche qui ont un commentaire (au
[format DocBlock][docblock] et qui contiennent les tags `@searchLabel` et
`@searchType`. Vous pouvez ainsi déclarer votre propre méthode, utilisable comme
critère de recherche, dans le fichier méthode de votre famille et ajouter les
commentaires DocBlock adéquats. Cette méthode est généralement statique car elle
ne doit pas faire appel à des valeurs de document. Par contre vous pouvez bien
entendu utiliser des paramètres de la famille. La valeur de retour de cette
méthode sera utilisé comme valeur du critère. Cela ne peut être appliqué que sur
des opérateurs nécessitant une seule valeur. Cette méthode spécifique sera
utilisable pour des recherches détaillées portant sur la famille en question.

Exemple de déclaration de méthode :

    [php]
    /**
     * Get a random integer between 1 and 10
     * 
     * @searchLabel Random integer between 1 and 10
     * @searchType int
     * @searchType double
     * @searchType money
     */
    public function getRandomNumber() {
        return mt_rand(1, 10);
    }

Le tag `@searchLabel` permet de spécifier le libellé qui sera présenté lors de
l'affichage de la liste des méthodes compatibles. Ce libellé sera traduit via le
mécanisme de [localisation][i18n].

Le tag `@searchType` permet de spécifier les types d'attributs sur lesquels
cette méthode est utilisable. L'interface de composition des critères de la
recherche détaillée ne présentera alors que les méthodes compatibles avec
l'attribut du critère.

Pour des besoins plus complexes de sélection des méthodes compatibles, vous
pouvez surcharger la méthode `Doc::getSearchMethods()` pour enlever ou ajouter
des méthodes à la liste générée par défaut. Les méthodes que vous ajoutez
devront aussi avoir les tags `@searchLabel` et `@searchType` positionnée.

Exemple de surcharge de `Doc::getSearchMethods` :

    [php]
    public function getSearchMethods($attrId, $attrType) {
        $methods = parent::getSearchMethods($attrId, $attrType);
        if ($attrType == 'date' || $attrType == 'timestamp') {
            /*
             * Ajouter avant-hier et après-demain
             * pour les attributs de type 'date' et 'timestamp'
             */
            $methods = array_merge(
                $methods,
                array(
                    array(
                        'label' => _("Day before yesterday"),
                        'method' => '::getDate(-2)'
                    ),
                    array(
                        'label' => _("Day after tomorrow"),
                        'method' => '::getDate(2)'
                    )
                )
            );
        }
        return $methods
    }


<!-- link -->
[docblock]:     http://www.phpdoc.org/docs/latest/for-users/anatomy-of-a-docblock.html
[i18n]:         #core-ref:8f3ad20a-4630-4e86-937b-da3fa26ba423