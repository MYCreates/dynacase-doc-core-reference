# createTmpDoc {#core-ref:6b745549-eb65-46f5-b0c1-5fa80661f1b7}

<div markdown="1" class="short-description">
Cette fonction permet de créer un nouveau document temporaire.
</div>

## Description {#core-ref:af2d35c2-4cbb-4e18-85cc-467ed8a65319}

    [php]
    [bool|Doc] createDoc ( string $dbaccess,
                           string $fromid, 
                             bool $defaultvalues = true)

Cette fonction permet de créer un nouveau document temporaire. Les documents 
temporaires stockés en base sont effacés par le script [`cleanContext`][cleancontext] .

### Avertissements  {#core-ref:d70154f2-ca8c-48b0-a2ea-68b040e1aa4b}

Les documents temporaires n'ont pas de :

* cycle de vie,
* profil,
* contrôle de vue.

Le document retourné n'est pas inséré en base.
Si le paramètre `fromid` ne correspond pas à un nom logique de famille un objet
de type `\Dcp\Family\Document` est retourné, cet objet n'est pas utilisable et
doit être considéré comme un retour d'erreur.

## Liste des paramètres  {#core-ref:aeae9a63-f567-4f1a-8950-10778027c098}

(string) `dbaccess`
:   Coordonnées de la base de données. Cet élément peut-être trouvé grâce à la
    fonction `getDbAccess`.

(string) `fromid`
:   Nom logique de la famille du document à créer.

(bool) `defaultvalues` (défaut : true)
:   Indique si les valeurs par défaut doivent être initialisées dans le document
    retourné.

## Valeur de retour  {#core-ref:f5ca55dc-7323-4d6f-b164-d03c4883e445}

Un document de la classe correspondant à la famille demandée.

Le document retourné n'est pas inséré en base, c'est à la charge du développeur
de procéder, si besoin, à cette insertion à l'aide de la méthode
[Doc::store()][store].

## Erreurs / Exceptions {#core-ref:f468f7e4-ef37-4c6c-b562-6d2a2e09e8c8}

Si le paramètre `fromid` ne correspond pas à un nom logique de famille un objet
de type `\Dcp\Family\Document` est retourné, cet objet n'est pas utilisable et
doit être considéré comme un retour d'erreur.

## Historique  {#core-ref:80c1cf22-74e7-4511-814a-4a6f4172e966}

Aucun

## Exemple  {#core-ref:014f33a3-c3ec-4fe9-8198-4f797cbaa44e}

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


## Notes  {#core-ref:0e1ef39f-932c-4886-a0be-48e1ac6f64f5}

Aucune

## Voir aussi  {#core-ref:23e2bb8b-fc07-4d7a-8869-4669472e65f3}

* [createDoc()][createDoc],
* [new_Doc()][new_Doc].

<!-- links -->

[store]:            #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[createDoc]:        #core-ref:9886581a-243a-4c78-8490-8fda2209fd93
[new_Doc]:          #core-ref:e978cbd1-5f54-4a06-a6be-f1c079c2d734
[wsh]:              #core-ref:bab8c1c9-fe71-4629-9773-5cd67a8693bf
[cleancontext]:     #core-ref:100b123b-da1a-45b4-848b-0622f3e09a40
