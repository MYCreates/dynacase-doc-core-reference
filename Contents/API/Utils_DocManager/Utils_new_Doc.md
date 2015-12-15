# new_Doc {#core-ref:e978cbd1-5f54-4a06-a6be-f1c079c2d734}

<div markdown="1" class="short-description">
Cette fonction permet de récupérer un document stocké en base. 
</div>

## Description {#core-ref:59e92d4c-dc5d-432e-a74b-375d88e66d4c}

    [php]
    \Dcp\Family\Document new_Doc ( string $dbaccess, 
                               int|string $id = "",
                                     bool $latest = false )

Cette fonction retourne un objet héritant de la classe `\Dcp\Family\Document`
en fonction de l'identifiant du document (paramètre `$id`).

Elle réalise les points suivants :

* récupération des données et des propriétés du documents,
* récupération de son profilage,
* instanciation d'un objet de la classe correspond à la famille du document.


### Avertissements {#core-ref:133b0170-c3d3-4d0b-a106-23f0693ed8e1}

Pour des raisons de compatibilité avec les versions précédentes de Dynacase, la
fonction `new_Doc()` retourne un objet de la classe `\Dcp\Family\Document` si
l'identifiant n'existe pas où n'est pas indiqué.

Cet objet n'est pas utilisable en tant que tel et doit être considéré comme un
retour d'erreur.

Cette méthode utilise l'objet de [partage de document][shareddoc]. Si le
document demandé est dans cet objet de partage, le document est récupéré de la
zone de partage et non de la base de données.

## Liste des paramètres {#core-ref:1f4c5acd-963a-4b13-95c8-8edd3cc783ff}

(string) `dbaccess`
:   Indique les coordonnées de la base de données. Ce paramètre est maintenant
    obsolète et peut être laissé vide.

(int|string) `id`
:   Identifiant du document que l'on souhaite récupérer. Cet identifiant est
    soit la [propriété][properties] `id` du document, soit la [propriété][properties] `name` 
    (nom logique) du document. Si l'identifiant désigne un nom logique, la révision 
    courante du document est retournée (l'option `latest` est ignorée).

(string) `latest` (défaut : `false`)
:   Dans le cas d'un document [révisé][review] indique si l'on souhaite récupérer
    la version courante (`true`) ou la version référencée par le paramètre `id`.

## Valeur de retour {#core-ref:eaf17066-d604-47b6-94f3-7c21f5f7fac4}

Un document héritant de la classe `\Dcp\Family\Document`, la classe exacte du 
document est fonction de la famille du document.

## Erreurs / Exceptions {#core-ref:14d36bfc-8388-48c9-845a-94c92452f645}

Si jamais, le fichier [méthode][method] PHP associé à la famille du document 
n'existe pas alors `null` est retourné.

Lorsque il n'existe pas de document correspondant au paramètre `$id` ou lorsque
le paramètre `$id` n'est pas passé alors un objet de la classe
`\Dcp\Family\Document` est retourné. Cet objet doit être considéré comme un
retour d'erreur et n'est pas utilisable, pour détecter ce type de retour vous
pouvez utiliser les fonctions :

* [`Doc::isAlive()`][isAlive] : cette méthode vérifie que l'objet existe en
    base et n'a pas été supprimé,

* `Doc::isAffected()` : cette méthode vérifie que l'objet existe en base.

## Historique {#core-ref:723bf281-45bd-46ad-bc1d-1917c6d05f79}

Aucun

## Exemple {#core-ref:a6344de2-7961-4715-b7b7-455a0498bf57}


Le document de test de nom logique `FF` a deux révisions avec les identifiants
1455 et 1460. Il s'agit d'un document de la famille "Demande d'adoption".
Le document 1461 de la famille "Animal" a été supprimé.

    [php]
    
    require_once "FDL/freedom_util.php";
    
    $docs = array();
    $docs["FF"] = new_Doc("", "FF");
    $docs[1455] = new_Doc("", 1455);
    $docs["1455_latest"] = new_Doc("", 1455, true);
    $docs[1461] = new_Doc("", 1461);
    $docs["idontexist"] = new_Doc("", "idontexist");
    $docs["withoutArgument"] = new_Doc();
    foreach ($docs as $key => $doc) {
        if ($doc->isAlive()) {
            printf("[%s]: Doc %s (%d : %s) is affected and alive\n", $key, $doc->getTitle(), $doc->getPropertyValue("id"), get_class($doc));
        } elseif ($doc->isAffected()) {
            printf("[%s]: Doc %s (%d : %s) is only affected\n", $key, $doc->getTitle(), $doc->getPropertyValue("id"), get_class($doc));
        } else {
            printf("[%s]: Doesn't exist (the document has the class %s)\n", $key, get_class($doc));
        }
    }

Résultat :

    [FF]: Doc ff (1460 : Dcp\Family\Zoo_demandeadoption) is affected and alive
    [1455]: Doc ff (1455 : Dcp\Family\Zoo_demandeadoption) is affected and alive
    [1455_latest]: Doc ff (1460 : Dcp\Family\Zoo_demandeadoption) is affected and alive
    [1461]: Doc supprimé (1461 : Dcp\Family\Zoo_animal) is only affected
    [idontexist]: Doesn't exist (the document has the class Dcp\Family\Document)
    [withoutArgument]: Doesn't exist (the document has the class Dcp\Family\Document)


## Notes {#core-ref:1fca9e08-062b-495b-a8a3-7561f57d43af}

Cette fonction ne doit pas être utilisée pour créer un nouveau document.

La création de document est réalisée par la fonction [createDoc()][createDoc].

## Voir aussi {#core-ref:f3de1eb0-942f-488b-a9dc-e165a7bf7c9f}

* [createDoc()][createDoc],
* [createTmpDoc()][createTmpDoc],
* [Doc::preAffect()][preaffect],
* [Partage de documents][shareddoc].

<!-- links -->

[properties]:   #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353
[review]:       #core-ref:8c947ef6-8da5-4fca-93cf-897c81ebb53d
[method]:       #core-ref:6f013eb8-33c7-11e2-be43-373b9514dea3
[isAlive]:      #core-ref:b791d14e-800a-4b3b-bee7-41e271a8087e
[createDoc]:    #core-ref:9886581a-243a-4c78-8490-8fda2209fd93
[createTmpDoc]: #core-ref:6b745549-eb65-46f5-b0c1-5fa80661f1b7
[preaffect]:    #core-ref:e6f36fea-9f42-4751-ba9b-c3aafec56559
[shareddoc]:    #core-ref:947948f6-242c-40a7-8f70-8013fe2ab1f1