# Dir::insertDocument() {#core-ref:9575ff95-480a-4dfb-9cd0-b89f44c3fad7}

<div class="short-description" markdown="1">

La méthode `insertDocument` permet d'insérer un document dans le dossier.

</div>

## Description {#core-ref:d46e6f49-f39a-4fe6-8851-38a2eb5be71a}

    [php]
    string insertDocument ( string $docid,
    	                    string $mode          = "latest",
    	                      bool $noprepost     = false,
    	                      bool $forcerestrict = false,
    	                      bool $nocontrol     = false )

Cette méthode permet d'ajouter une référence au dossier. Une référence
correspond à un document sans tenir compte de ses [révisions][revision]. La
référence correspond à l'identifiant initial du document (`initid`).

### Avertissements {#core-ref:a60c6daf-c65a-4d40-b770-102166a3c45d}

Si la référence existe déjà dans le dossier, la référence est ignorée.



## Liste des paramètres {#core-ref:f4d478ce-8145-4975-801e-d6158cb8b7d2}

(string) `docid`
:   L'identifiant (identifiant numérique ou nom logique) du document à insérer
    dans le dossier. Cet identifiant peut correspondre à n'importe quelle 
    [révision][revision] du document. 

(string) `mode`
 :  Seule la valeur `latest` (valeur par défaut) est supportée.

(bool) `noprepost`
:   `noprepost` permet de désactiver l'appel des méthodes hameçons
    [`preInsertDocument`][Dir::preInsertDocument] et
    [`postInsertDocument`][Dir::postInsertDocument] appelées respectivement
    avant et après l'insertion du document dans le dossier.
    
    Valeur par défaut : `false` (activation des hameçons).
    
    Si `false` les hameçons [`preInsertDocument`][Dir::preInsertDocument] et
    [`postInsertDocument`][Dir::postInsertDocument] sont appelés.  
    Si `true`, les hameçons ne sont pas appelés.

(bool) `forcerestrict`
:   `forcerestrict` permet de désactiver le contrôle des
    [restrictions][restrictions] d'insertion.
    
    Valeur par défaut : `false` (contrôle des restrictions effectuée).
    
    Si `false` : le contrôle des [restrictions][restrictions] d'insertion est
    effectué.  
    Si `true` : le contrôle des restrictions d'insertion n'est pas effectué.

(bool) `nocontrol`
:   `nocontrol` permet de désactiver le [contrôle du droit][pdir] d'insertion de
    documents dans le dossier.
    
    Valeur par défaut : `false` (contrôle de droits effectué).
    
    Si `false` : le contrôle du droit d'insertion est effectué.  
    Si `true` : le contrôle du droit d'insertion n'est pas effectué.

## Valeur de retour {#core-ref:38a6159e-800d-4314-92ee-73ba7a07e910}

La méthode retourne une chaîne vide s'il n'y a pas d'erreurs, ou une chaîne non-
vide contenant le message d'erreur dans le cas contraire.

## Erreurs / Exceptions {#core-ref:79a2d856-79b5-4b68-9756-e710e4159e69}

Les erreurs peuvent êtres :

* L'utilisateur ne possède pas le droit d'insertion ([droit `modify`][docacl]) 
  de documents dans le dossier
* Le dossier est verrouillé par un autre utilisateur.
  (voir paramètre `nocontrol` ci-dessus).
* Le document inséré n'est pas compatible par rapport aux
  [restrictions][restrictions] appliqués sur le dossier (voir paramètre
  `forcerestrict` ci-dessus).
* Les méthodes d'hameçon [`preInsertDocument`][Dir::preInsertDocument] ou
  [`postInsertDocument`][Dir::postInsertDocument] ont retourné une erreur (voir
  paramètre `noprepost` ci-dessus).

## Historique {#core-ref:88acd043-c1d4-48bb-9d87-5f3726fa69da}

### Release 3.2.5 {#core-ref:c0d860d4-afdb-4978-a06c-6a598e36180b}

La méthode `insertDocument` remplace la méthode précédemment nommée `addFile`.

L'utilisation de `addFile` est obsolète depuis la version 3.2.5 de dynacase-
core.

## Exemples {#core-ref:2ef6f6b5-f383-4c1d-8af6-9ed63c974d37}

La famille _Groupe d'utilisateurs_ (`IGROUP`) hérite de `GROUP` qui hérite de
`DIR` et permet de gérer l'affectation d'utilisateurs dans les groupes.

L'exemple ci-dessous montre comment insérer l'utilisateur ayant pour nom logique
`USR_EMMA_PEEL` dans le groupe ayant pour nom logique `GRP_THE_AVENGERS` :

    [php]
    <?php
    
    global $action;
    
    include_once 'FDL/freedom_util.php';
    
    $grpName = 'GRP_THE_AVENGERS';
    $usrName = 'USR_EMMA_PEEL';
    
    /*
     * Instancier le groupe
     */
    
    $group = \Dcp\DocManager::getDocument( $grpName);
    
    /*
     * Instancier l'utilisateur
     */
    
    $user = \Dcp\DocManager::getDocument( $usrName);
    
    /*
     * Afficher le contenu du groupe
     */
    
    printf("* Content of DIR '%s':\n", $grpName);
    $s = new SearchDoc();
    $s->useCollection($group->id);
    $members = $s->search();
    if (count($members) <= 0) {
        printf("\t<empty>\n");
    } else {
        foreach ($members as $elmt) {
                printf("\t(%d) '%s'\n", $elmt['id'], $elmt['title']);
        }
    }
    
    /*
     * Insérer l'utilisateur dans le groupe
     */
    
    printf("* Inserting '%s' into '%s'.\n", $usrName, $grpName);
    $err = $group->insertDocument($user->id);
    if ($err != '') {
        throw new \Exception(sprintf("Error inserting user (%d) '%s' into group (%d) '%s': %s", $user->id, $user->title, $group->id, $group->title, $err));
    }
    
    /*
     * Afficher le contenu du groupe
     */
    printf("* Content of DIR '%s':\n", $grpName);
    $s = new SearchDoc();
    $s->useCollection($group->id);
    $members = $s->search();
    if (count($members) <= 0) {
        printf("\t<empty>\n");
    } else {
        foreach ($members as $elmt) {
                printf("\t(%d) '%s'\n", $elmt['id'], $elmt['title']);
        }
    }

Résultat :

    * Content of DIR 'GRP_THE_AVENGERS':
            <empty>
    * Inserting 'USR_EMMA_PEEL' into 'GRP_AVENGERS'.
    * Content of DIR 'GRP_THE_AVENGERS':
            id = 1057 / title = 'Peel Emma'

## Notes {#core-ref:c8699f3a-204d-49a0-a9b1-ae99815ec9cd}

Ordre d'appel des hameçons :

    .-(1)--------------------------------------.
    | $this->preInsertDocument($docId, false); |
    '------------------------------------------'    
    
    <Insertion du document $docId dans le dossier>
    
    .-(2)---------------------------------------.
    | $this->postInsertDocument($docId, false); |
    '-------------------------------------------'

## Voir aussi {#core-ref:f72b472a-0d7d-4be2-9dee-1cd6776b2791}

- [`Dir::preInsertDocument`][Dir::preInsertDocument]
- [`Dir::postInsertDocument`][Dir::postInsertDocument]
- [`Dir::removeDocument`][Dir::removeDocument]
- [`Dir::insertMultipleDocuments`][Dir::insertMultipleDocuments]

<!-- links -->
[Dir::preInsertDocument]: #core-ref:2f9580d7-cd06-4d09-8853-ed95f614d665
[Dir::postInsertDocument]: #core-ref:65ec2b4a-8878-4004-8e42-0de8c359a231
[Dir::removeDocument]: #core-ref:d337e186-8066-49e2-92a0-26aa518cbf41
[Dir::postInsertMultipleDocuments]: #core-ref:e3cd509f-8678-4dec-a0cf-33aa39674cfe
[restrictions]: #core-ref:ad55c0a7-fc0f-4c9d-95cb-8286f4057c3f
[Dir::insertMultipleDocuments]: #core-ref:098cf44e-568d-4dd2-8dd0-e2f104bc8615
[docacl]:       #core-ref:f1575705-10e8-4bf2-83b3-4c0b5bfb77cf
[revision]:     #core-ref:6b3d41c4-e94c-41ab-adbc-51069ab7119d
[pdir]:                     #core-ref:0cd3fe9a-57cf-481f-8fc0-560bc71d6430
