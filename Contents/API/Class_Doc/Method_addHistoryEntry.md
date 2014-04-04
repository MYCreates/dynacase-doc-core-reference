# Doc::addHistoryEntry() {#core-ref:2f1828bf-b312-4996-9a56-42ac79e7dbf3}

<div class="short-description">
Ajoute un message dans l'historique du document.
</div>

## Description {#core-ref:6770b284-8740-4ba1-8c9d-cc4354f6a7c6}

    [php]
    string addHistoryEntry ( string $comment = '', 
                                int $level = DocHisto::INFO,
                             string $code = '',
                                int $uid = '')

Cette méthode permet d'enregistrer un nouveau message daté dans l'historique du
document.

### Avertissements {#core-ref:9915015f-7e30-46c0-bb10-19898db22f98}

Les messages enregistrés ne sont pas localisables. Cela implique que les messages
restitués par l'historique ne sont pas traduits.

## Liste des paramètres {#core-ref:293eee10-486a-4216-a18c-9fae74e58407}


(string) `comment`
:   Message qui sera enregistré dans l'historique.

(int) `level`
:   Niveau du message :
    
    *   `DocHisto::NOTICE` : notification,
    *   **`DocHisto::INFO`** : message d'information (valeur par défaut),
    *   `DocHisto::MESSAGE` : message standard,
    *   `DocHisto:: WARNING` : message d'avertissement,
    *   `DocHisto::ERROR` : message d'erreur. Dans ce cas, le message est aussi 
        ajouté au fichier log d'erreur du serveur _Apache_.

(string) `code` 
:   Code du message : ce code peut être utilisé pour répertorier certaines
    entrées dans l'historique. Par exemple, le code `CREATE` est utilisé pour
    indiquer les messages relatifs à la création d'un document. La méthode
    `Doc::getHisto()` permet de filtrer en fonction du code les messages de
    l'historique.

(int) `uid`
:   Identifiant système du compte utilisateur. Si celui-ci est vide alors le
    message sera attribué à l'utilisateur courant.

## Valeur de retour {#core-ref:4c31f31d-3302-4dc0-81eb-9bb18af3738d}

Message d'erreur.

## Erreurs / Exceptions {#core-ref:2106e286-3ec5-4292-bc0b-409daa910df1}

Une exception `Dcp\Db\Exception` peut être levée en cas de problème
d'enregistrement.

## Historique {#core-ref:4d74b50e-414f-4d74-a830-854229e6817e}

Cette méthode était anciennement nommée `addComment`.

## Exemples {#core-ref:8713f08c-bdc4-428c-a671-dc025f82f013}

Enregistrement d'un nouveau message d'avertissement.

    [php]
    /** @var \Dcp\Family\MyFamily */
    $d = \Dcp\DocManager::getDocument( "MY_DOCUMENT");
    if ($d !== null) {
        // enregistrement
        $d->addHistoryEntry("Hello world", DocHisto:: WARNING);
    }


## Notes {#core-ref:24d7ea33-d93d-4643-9958-04712d091a59}

Il n'y a pas de méthode depuis la classe `Doc` permettant de supprimer des
messages de l'historique. Pour manipuler et gérer l'historique du document, il
faut utiliser la classe `DocHisto`.

## Voir aussi {#core-ref:0a8de26a-d296-485c-a78e-ef89220a00f1}

Aucun.
