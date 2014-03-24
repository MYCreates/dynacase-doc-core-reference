# createTmpDoc {#core-ref:6b745549-eb65-46f5-b0c1-5fa80661f1b7}

<div markdown="1" class="short-description">
Cette fonction permet de créer un nouveau document temporaire.
</div>

## Description {#core-ref:83d5ce37-f73b-4a4a-8e14-8838c39732c3}

    [php]
    [bool|Doc] createDoc ( string $dbaccess,
                           string $fromid, 
                             bool $defaultvalues = true)

Cette fonction permet de créer un nouveau document temporaire. Les documents 
temporaires stockés en base sont effacés par le script [`cleanContext`][cleancontext] .

### Avertissements {#core-ref:ffd97441-2cec-4f71-a201-6f0c1d2f3e3c}

Les documents temporaires n'ont pas de :

* cycle de vie,
* profil,
* contrôle de vue.

Le document retourné n'est pas inséré en base.
Si le paramètre `fromid` ne correspond pas à un nom logique de famille un objet
de type `\Dcp\Family\Document` est retourné, cet objet n'est pas utilisable et
doit être considéré comme un retour d'erreur.

## Liste des paramètres {#core-ref:7d07e2fd-c2c2-4c69-8299-5b5b22322fa7}

(string) `dbaccess`
:   Coordonnées de la base de données. Cet élément peut-être trouvé grâce à la
    fonction `getDbAccess`.

(string) `fromid`
:   Nom logique de la famille du document à créer.

(bool) `defaultvalues` (défaut : true)
:   Indique si les valeurs par défaut doivent être initialisées dans le document
    retourné.

## Valeur de retour {#core-ref:250a7302-7feb-44b6-b545-e66ef65328b6}

Un document de la classe correspondant à la famille demandée.

Le document retourné n'est pas inséré en base, c'est à la charge du développeur
de procéder, si besoin, à cette insertion à l'aide de la méthode
[Doc::store()][store].

## Erreurs / Exceptions {#core-ref:a1b1854a-d854-4b33-a776-b3f76419f697}

Si le paramètre `fromid` ne correspond pas à un nom logique de famille un objet
de type `\Dcp\Family\Document` est retourné, cet objet n'est pas utilisable et
doit être considéré comme un retour d'erreur.

## Historique {#core-ref:846b4bf9-7e55-4f6b-9ec0-f7b67af3674a}


<span class="flag release obsolete">3.3.0</span>
Remplacé par [`Dcp\DocManager::createTemporariDoc()`][createtmpdocument].

## Exemple {#core-ref:c94038b0-45ac-4ed1-8ff1-a83d32d3dfa2}

    [php]
    
    require_once "FDL/freedom_util.php";
    
    $animal = createTmpDoc(getDbAccess(), "ZOO_ANIMAL");
    
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

* [createDoc()][createDoc],
* [new_Doc()][new_Doc].

<!-- links -->

[store]:            #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[createDoc]:        #core-ref:9886581a-243a-4c78-8490-8fda2209fd93
[new_Doc]:          #core-ref:e978cbd1-5f54-4a06-a6be-f1c079c2d734
[wsh]:              #core-ref:bab8c1c9-fe71-4629-9773-5cd67a8693bf
[cleancontext]:     #core-ref:100b123b-da1a-45b4-848b-0622f3e09a40
[createtmpdocument]:    #core-ref:5e5cda73-398a-49fd-a14f-24ee877cf3fa