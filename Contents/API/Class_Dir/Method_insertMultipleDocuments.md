# Dir::insertMultipleDocuments() {#core-ref:098cf44e-568d-4dd2-8dd0-e2f104bc8615}

<div class="short-description" markdown="1">

La méthode `insertMultipleDocuments` permet d'insérer plusieurs documents dans
le même dossier.

</div>

## Description {#core-ref:7e1333fe-8887-444b-a780-6b5d346bba5c}

    [php]
    string insertMultipleDocuments ( array $tdocs,
                                    string $mode      = "latest",
                                      bool $noprepost = false,
                                         & $tinserted = array(),
                                         & $twarning  = array(),
                                         & $info      = array() )
    )

### Avertissements {#core-ref:486fcbf1-dbb9-425d-8980-2a978c58d422}

Aucun.

## Liste des paramètres {#core-ref:511b14bf-eb70-4b67-9205-e75cc110696a}

(array) `tdocs`
:   `tdocs` est une liste de documents bruts (voir
    [Retour de documents bruts][SearchDoc_bruts] avec [`SearchDoc`][SearchDoc])
    à insérer dans le dossier.

(string) `mode`
:  Seule la valeur `latest` (valeur par défaut) est supportée.

(bool) `noprepost`
:   `noprepost` permet de désactiver l'appel des méthodes hameçons
    [`preInsertMultipleDocuments`][Dir::preInsertMultipleDocuments],
    [`preInsertDocument`][Dir::preInsertDocument],
    [`postInsertDocument`][Dir::postInsertDocument] et
    [`postInsertMultipleDocuments`][Dir::postInsertMultipleDocuments] appelées
    avant ou après l'insertion des documents dans le dossier.
    
    Valeur par défaut : `false`.
    
    Si `false`, les hameçons
    [`preInsertMultipleDocuments`][Dir::preInsertMultipleDocuments],
    [`preInsertDocument`][Dir::preInsertDocument] et
    [`postInsertMultipleDocuments`][Dir::postInsertMultipleDocuments] sont
    appelés.

[out] (string[]) `tinserted`
:   `tinserted` référence un array associatif retourné par la méthode.
    
    Les clés sont les identifiants (`initid`) des documents insérés dans le
    dossier et la valeur est un message indiquant que le document a été inséré
    dans le dossier.
    
    Exemple de valeur retournée dans le array référencé :
    
        [php]
        array(
            1234 => 'Document 1234 inséré',
            2345 => 'Document 2345 inséré'
        )

[out] (string[]) `twarning`
:   `twarning` référence un array associatif retourné par la méthode.
    
    Les clés sont les identifiants des documents qui n'ont pas pu être insérés
    dans le dossier et la valeur est le message d'erreur de l'insertion.

[out] (array) `info`
:   `info` référence un array associatif retourné par la méthode.
    
    Il contient les erreurs retournés par les différents hameçons, et est de la
    forme suivante :
    
        [php]
        array(
            'error' => "Message d'erreur global de insertMultipleDocuments",
            'modifyError' => "Erreur lié au droit de modification"
            'preInsertMultipleDocuments' => "Message d'erreur de preInsertMultipleDocuments",
            'preInsertDocument'           => array(
                "Message d'erreur de preInsertDocument#1",
                ...
                "Message d'erreur de perInsertDocument#N"
                ),
            'postInsertDocument'          => array(
                "Message d'erreur de postInsertDocument#1",
                ...
                "Message d'erreur de postInsertDocument#N"
                ),
            'postInsertMultipleDocuments' => "Message d'erreur de postInsertMultipleDocuments()"
        )

## Valeur de retour {#core-ref:5c227a9d-1932-4cca-b466-12e14e1357da}

La méthode retourne une chaîne vide s'il n'y a pas d'erreurs, ou une chaîne non-
vide contenant le message d'erreur.

## Erreurs / Exceptions {#core-ref:dd396262-a036-4587-ab07-1c0ddca80c66}

Les erreurs peuvent êtres :

* L'utilisateur ne possède pas le droit d'insertion de documents dans le dossier.
* Le dossier est verrouillé par un autre utilisateur.
* Le document inséré n'est pas compatible par rapport aux documents
  supportés par le dossier.
* Les méthodes d'hameçon
  [`preInsertMultipleDocuments`][Dir::preInsertMultipleDocuments],
  [`preInsertDocument`][Dir::preInsertDocument],
  [`postInsertDocument`][Dir::postInsertDocument] ou
  [`postInsertMultipleDocuments`][Dir::postInsertMultipleDocuments] ont retourné
  une erreur (voir paramètre `noprepost` ci-dessus).

Les documents qui n'ont pas pu être insérés dans le dossier peuvent être
consultés via l'argument retour `$twarning` passé à la méthode
[`insertMultipleDocuments`][Dir::insertMultipleDocuments].

## Historique {#core-ref:d1689444-f36d-467b-bdfe-3fd001cd2bf7}

### Release 3.2.5 {#core-ref:fb3d2fa4-3efd-4f83-bff3-ee6f1cd9adda}

La méthode `insertMultipleDocuments` remplace la méthode précédemment nommée
`InsertMDoc`.

L'utilisation de `InsertMDoc` est obsolète depuis la version 3.2.5 de dynacase-
core.

### Release 3.2.12 {#core-ref:151fed1c-a8fe-4d80-84f6-a3239f9d903b}

<span class="flag from release">3.2.12</span> La méthode
`Dir::insertMultipleDocuments` a été modifiée afin de faire remonter le message
d'erreur de la méthode hameçon `Dir::postInsertMultipleDocuments` dans son
retour d'erreur.

La méthode `Dir::insertMultipleDocuments` ajoute un 6ème paramètre optionnel
[`info`][Dir::insertMultipleDocuments_info] qui permet de récupérer les
différents messages d'erreurs rencontrés.

## Exemples {#core-ref:03752194-67ae-4978-9236-e352f0db2d40}

L'exemple suivant va rechercher tous les utilisateurs dont l'adresse mail
contient  `@the-avengers.net`, et les insérer dans un nouveau groupe
`GRP_THE_AVENGERS` :

    [php]
    include_once "FDL/freedom_util.php";
    
    /*
     * Créer un nouveau groupe GRP_EXAMPLE_NET
     */
     
    /**
     * @var \dcp\Family\Igroup $group
     */
    $group = \Dcp\DocManager::createDocument('IGROUP');
    $group->setAttributeValue('us_login', 'grp_the_avengers');
    $group->setAttributeValue('grp_name', 'Groupe @the-avengers.net');
    $group->store();
    
    /*
     * Affecter un nom logique au groupe
     */
     
    $group->setLogicalName('GRP_THE_AVENGERS');
    
    /*
     * Rechercher tous les utilisateurs ayant une
     * adresse mail en "@the-avengers.net".
     */
     
    $s = new SearchDoc('', 'IUSER');
    $s->addFilter("us_extmail LIKE '%@the-avengers.net'");
    $userList = $s->search();
    if (count($userList) > 0) {
        /*
         * Insérer tous ces utilisateurs dans le groupe.
         */
         
        $insertedList = array();
        $warningList = array();
        $err = $group->insertMultipleDocuments(
            $userList,
            "latest",
            false,
            $insertedList,
            $warningList
        );
        if ($err != '') {
            printf("* Some documents have not been inserted: %s", $err);
        }
    printf("* %d insertions :\n", count($insertedList));
    foreach ($insertedList as $docId => $msg) {
        printf("\t[%s] %s\n", $docId, $msg);
    }
    printf("* %d warnings :\n", count($warningList));
    foreach ($warningList as $docId => $warningMsg) {
        printf("\t[%d] %s\n", $docId, $warningMsg);
    }
}

Résultat :

    * 2 insertions :
            [1057] Insertion document Peel Emma 
            [1058] Insertion document Steed John 
    * 0 warnings :
    

## Notes {#core-ref:616462ed-1e19-4078-a825-be892220b00c}

Ordre d'appel des hameçons :

    .-(1)-------------------------------------------------------.
    | $this->preInsertMultipleDocuments($rawDocumentList);      |
    '-----------------------------------------------------------'
    
    foreach ($rawDocumentList as $rawDoc) {
    
        .-(2)--------------------------------------.
        | $this->preInsertDocument($rawDoc, true); |
        '------------------------------------------'
    
        <Insertion du document $rawDoc['id'] dans le dossier>
    
        .-(3)---------------------------------------.
        | $this->postInsertDocument($rawDoc, true); |
        '-------------------------------------------'
    
    }
    
    .-(4)-------------------------------------------------------.
    | $this->postInsertMultipleDocuments($rawDocumentList);     |
    '-----------------------------------------------------------'

## Voir aussi {#core-ref:ebdafc6b-6857-4a46-b507-a74cc477889b}

- [`Dir::preInsertMultipleDocuments`][Dir::preInsertMultipleDocuments]
- [`Dir::preInsertDocument`][Dir::preInsertDocument]
- [`Dir::postInsertDocument`][Dir::postInsertDocument]
- [`Dir::postInsertMultipleDocuments`][Dir::postInsertMultipleDocuments]
- [`Dir::insertDocument`][Dir::insertDocument]
- [`Dir::removeDocument`][Dir::removeDocument]

<!-- links -->
[Dir::insertMultipleDocuments]: #core-ref:098cf44e-568d-4dd2-8dd0-e2f104bc8615
[Dir::preInsertMultipleDocuments]: #core-ref:f2b05730-8eca-43de-a9f4-6075c7f19d66
[Dir::preInsertDocument]: #core-ref:2f9580d7-cd06-4d09-8853-ed95f614d665
[Dir::postInsertDocument]: #core-ref:65ec2b4a-8878-4004-8e42-0de8c359a231
[Dir::postInsertMultipleDocuments]: #core-ref:e3cd509f-8678-4dec-a0cf-33aa39674cfe
[Dir::insertDocument]: #core-ref:9575ff95-480a-4dfb-9cd0-b89f44c3fad7
[Dir::removeDocument]: #core-ref:d337e186-8066-49e2-92a0-26aa518cbf41
[SearchDoc_bruts]: #core-ref:4c508940-f5a0-40ee-a942-6372a95d112e
[SearchDoc]: #core-ref:7291dea8-a2db-46be-8194-bc6f100cc467
[Dir::insertMultipleDocuments_info]: #core-ref:511b14bf-eb70-4b67-9205-e75cc110696a
[pdir]:                     #core-ref:0cd3fe9a-57cf-481f-8fc0-560bc71d6430
