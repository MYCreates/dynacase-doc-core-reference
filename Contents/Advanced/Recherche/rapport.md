# Rapports {#core-ref:32bc0b13-2b9f-4096-ac83-15f2b40d3b39}

Les rapports (famille `REPORT`) héritent de la famille recherche détaillée
(famille `DSEARCH`).

## Création d'un rapport {#core-ref:dc0f4a5c-272e-4084-b8a8-3fab403c6eb4}

### Critères de recherche {#core-ref:3abd5d36-d4d5-43e3-9886-95bbadeb9aeb}

L'enregistrement de la requête est identique à celle de la [recherche
détaillée][dsearch].

    [php]
    use \Dcp\AttributeIdentifiers as Attribute;
    
    /**
     * @var \Dcp\Family\Report $report
     */
    $report = \Dcp\DocManager::createDocument(\Dcp\Family\Report::familyName);
    
    $report->setAttributeValue(Attribute\Report::ba_title, "Report With toview Tag");
    $report->setAttributeValue(Attribute\Report::se_famid, \Dcp\Family\Iuser::familyName);
    $report->setAttributeValue(Attribute\Report::ba_title, "Report on Active Users with email");
    $criteria = array(
        array(Attribute\Report::se_attrids => Attribute\Iuser::us_status,
              Attribute\Report::se_funcs => "=",
              Attribute\Report::se_keys => "A"),
        array(Attribute\Report::se_attrids => Attribute\Iuser::us_extmail,
              Attribute\Report::se_funcs => "is not null",
              Attribute\Report::se_keys => "")
    );
    $report->setAttributeValue(Attribute\Report::se_t_detail, $criteria);

## Présentation du rapport {#core-ref:f8c49fee-84a2-4d98-9eb3-0392fecee533}

Les colonnes à afficher sont indiquées par l'attribut `rep_idcols`. Cet attribut
doit contenir un identifiant d'attribut ou de propriété.

    [php]
    $columns = array(
        array(Attribute\Report::rep_lcols => "Utilisateurs",
              Attribute\Report::rep_idcols => "title",
              Attribute\Report::rep_foots => "CARD"),
        array(Attribute\Report::rep_lcols => "Utilisateurs",
              Attribute\Report::rep_idcols => Attribute\Iuser::us_whatid),
        array(Attribute\Report::rep_lcols => "Courriel",
              Attribute\Report::rep_idcols => Attribute\Iuser::us_mail),
        array(Attribute\Report::rep_lcols => "Groupes",
              Attribute\Report::rep_idcols => Attribute\Iuser::us_idgroup),
        array(Attribute\Report::rep_lcols => "Identifiant groupes",
              Attribute\Report::rep_displayoption => "docid",
              Attribute\Report::rep_idcols => Attribute\Iuser::us_idgroup)
    );
    
    $report->setAttributeValue(Attribute\Report::rep_tcols, $columns);
    $report->setAttributeValue(Attribute\Report::rep_style, "standard1");
    $report->store();

L'attribut `rep_lcols` (libellé) n'est pas utilisé lors de la génération. Le
libellé affiché correspond au libellé de l'attribut ou de la propriété.

Pour le cas particulier des attribut de [type `docid`][attdocid] et de [type
`account`][attaccount], si l'attribut `rep_displayoption` vaut `docid`, alors
ce sont les identifiants des documents qui sont affichés au lieu des titres des
documents.

<!-- link -->
[searchdoc]:        #core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255
[dsearch]:          #core-ref:7c36fc88-d5bc-4e0c-8cbf-092916168575 "Création d'une recherche détaillée"
[attdocid]:         #core-ref:d461d5f5-b635-47a0-944d-473c227587ab
[attaccount]:       #core-ref:87230967-8155-421c-8bbe-70a3c1adc3c0