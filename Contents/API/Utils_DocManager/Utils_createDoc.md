# createDoc {#core-ref:9886581a-243a-4c78-8490-8fda2209fd93}

<div markdown="1" class="short-description">
Cette fonction permet de créer un nouveau document.
</div>

## Description {#core-ref:83d5ce37-f73b-4a4a-8e14-8838c39732c3}

    [php]
    [bool|Doc] createDoc ( string $dbaccess,
                           string $fromid, 
                             bool $control = true, 
                             bool $defaultvalues = true, 
                             bool $temporary = false )

Cette fonction permet de créer un nouveau document. Elle :

* contrôle que la famille de document demandée existe,
* contrôle que l'utilisateur courant a le droit de créer ce type de document (optionnel),
* initialise un objet de la classe correspondant à la famille demandée,
* le paramètre avec les valeurs par défaut (optionnel).

Le document retourné n'est pas inséré en base, c'est à la charge du développeur
que de procéder, si besoin est, à cette insertion à l'aide de la méthode [store][store].

### Avertissements {#core-ref:ffd97441-2cec-4f71-a201-6f0c1d2f3e3c}

Le document retourné n'est pas inséré en base.

Si le paramètre `fromid` ne correspond pas à un nom logique de famille un objet
de type `\Dcp\Family\Document` est retourné, cet objet n'est pas utilisable et
doit être considéré comme un retour d'erreur.

## Liste des paramètres {#core-ref:7d07e2fd-c2c2-4c69-8299-5b5b22322fa7}

(string) `dbaccess`
:   Coordonnées de la base de données. Cette élément peut-être trouvé grâce à la
    fonction `getDbAccess()`.

(string) `fromid`
:   Nom logique de la famille du document à créer.

(bool) `control` (défaut : true)
:   Indique si la fonction doit vérifier si l'utilisateur courant à le droit de
    créer des documents de cette famille. Par défaut le droit est vérifié.

(bool) `defaultvalues` (défaut : true)
:   Indique si les valeurs par défaut doivent être initialisées dans le document
    retourné.

(bool) `temporary` (défaut : false)
:   Ce paramètre est obsolète et **ne doit pas** être utilisé. Pour créer un document
    temporaire, il faut utiliser la focntion [createTmpDoc()][createTmpDoc].

## Valeur de retour {#core-ref:250a7302-7feb-44b6-b545-e66ef65328b6}

Un document de la classe correspondant à la famille demandée.

Le document retourné n'est pas inséré en base, c'est à la charge du développeur
que de procéder, si besoin est, à cette insertion à l'aide de la méthode [store][store].

## Erreurs / Exceptions {#core-ref:a1b1854a-d854-4b33-a776-b3f76419f697}

Si le paramètre `fromid` ne correspond pas à un nom logique de famille un objet
de type `\Dcp\Family\Document` est retourné, cet objet n'est pas utilisable et
doit être considéré comme un retour d'erreur.
Si l'utilisateur n'a pas le droit de créer le document et que le contrôle est
effectué alors `false` est retourné.

## Historique {#core-ref:846b4bf9-7e55-4f6b-9ec0-f7b67af3674a}

<span class="flag release obsolete">3.3.0</span>
Remplacé par [`Dcp\DocManager::createDocument()`][createdocument].

## Exemple {#core-ref:c94038b0-45ac-4ed1-8ff1-a83d32d3dfa2}

    [php]
    require_once "FDL/freedom_util.php";
    
    $animal = createDoc(getDbAccess(), "ZOO_ANIMAL");
    
    var_export(get_class($animal));
    print("\n");
    printf("is alive ? %s", var_export($animal->isAlive(), true)."\n");
    $animal->setValue("an_espece", "ZOO_ESP_ALLI");
    $err = $animal->store();
    if ($err) {
        print $err;
    }
    printf("is alive ? %s", var_export($animal->isAlive(), true)."\n");


Résultat :

    'Dcp\\Family\\Zoo_animal'
    is alive ? false
    is alive ? true


## Notes {#core-ref:47db739d-510e-4a0c-aac8-b1ccc8d97237}

Aucune

## Voir aussi {#core-ref:5762458e-0672-4dd7-b50a-e2c469f5ff97}

* [createTmpDoc()][createTmpDoc],
* [new_Doc()][new_Doc].

<!-- links -->

[store]:            #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[createTmpDoc]:     #core-ref:6b745549-eb65-46f5-b0c1-5fa80661f1b7
[new_Doc]:          #core-ref:e978cbd1-5f54-4a06-a6be-f1c079c2d734
[createdocument]:   #core-ref:2f5afd12-1db3-4c69-a0fa-4b7fb044b723