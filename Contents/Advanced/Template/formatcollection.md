# Mise en forme d'une collection de documents {#core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81}

## Collection de documents {#core-ref:fc3d2418-7471-439e-a6d9-dcd30de9fc83}

Les collections de documents sont, comme évoqués dans le chapitre
 [d'introduction][intro_collection], des ensembles de 
documents statiques (répertoires) ou dynamiques (recherches).

## Classe de formatage {#core-ref:26703b3f-46d4-4b84-882b-3520da6b408e}

La classe `FormatCollection` facilite la mise en forme de ces ensembles de
documents pour leur utilisations ultérieures (exportation ou interface homme-
machine).


## Liste des paramètres entrants {#core-ref:3ce00d98-5576-467e-bc08-8e36fe47421b}

Le classe prend en entrée des objets [`DocumentList`][ClassDocumentList]

Exemple pour le formatage des documents 9 et 12.

    [php]
    $documentList=new DocumentList();
    $documentList->addDocumentIdentifiers(array(9,12)); // 
    
    $formatCollection =new FormatCollection();
    $formatCollection->useCollection($documentList);
    $render=$formatCollection->render();

<span class="flag inline nota-bene"/>  : les objets
[`DocumentList`][ClassDocumentList] peuvent être initialisés soit avec des id,
soit avec des objets [`SearchDoc`][ClassSearchDoc].

## Sortie {#core-ref:da2ae3f0-af0c-4747-92e6-0999f8f05ffe}

Le résultat du "render" donne un tableau avec une entrée par document formaté.
Par défaut seul le titre et l'identifiant (`id`) sont retournés, aucun attribut
n'est utilisé. Chaque entrée contient un champ "`properties`" indiquant les
propriétés et un champ "`attributes`" contenant les attributs. Ce dernier est
vide si aucun attribut n'a été indiqué dans le formatage.

    [php]
    Array 
    ( 
        [0] => Array 
            ( 
                ["properties"] => Array 
                    ( 
                        ["id"] => 12
                        ["title"] => "les profils" 
                    ) 
            ) 
        [1] => Array 
            ( 
                ["properties"] => Array 
                    ( 
                        ["id"] => 9 
                        ["title"] => "Racine" 
                    ) 
            ) 
    ) 

<span class="flag from release inline">3.2.18</span> Les documents ayant des
attributs en [visibilité][visibility] `I` ne sont pas exportés par défaut. La
méthode `::setVerifyAttributeAccess(false)` permet d'exporter sans tenir compte
de la visibilité `I`.

## Formatage des propriétés {#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893}

Les propriétés accessibles sont les suivantes :

**les propriétés brutes** : ensemble des [propriétés][propriete_doc] d'un document,

**les propriétés formatées** : 

* `url` : URL d'accès en visualisation du document,
* `family` : informations sur la famille du document, cette structure contient le 
    * titre de la famille (`title`), 
    * le nom logique (`name`), 
    * l'identifiant (numérique) (`id`),
    * l'adresse de téléchargement de l'icône (`icon`),
* `creationDate` : date de création de la lignée documentaire,
* `createdBy` : informations sur le créateur de la lignée documentaire, cette structure contient
    * le nom du créateur (`title`), 
    * le nom logique du document lié au compte du créateur (`name`), 
    * l'identifiant (numérique) (`id`),
    * l'adresse de téléchargement de l'icône (`icon`) du document représentant le créateur,
* `revisionData` : informations sur la révision du document, cette structure contient : 
    * un booléen indiquant si le document a été modifié (`isModified`) depuis sa dernière révision, 
    * son identifiant (`id`), 
    * son numéro de révision (`number`),
    * les informations sur le créateur de la révision (`createdBy`), 
      cette structure est similaire à `createdBy`,
* `viewController` : informations sur la vue en cours d'application, cette structure contient 
    * l'identifiant du contrôle de vue (`id`),
    * son titre (`title`),
* `workflow` : informations sur le cycle de vie associé au document, cette structure contient 
    * l'identifiant (`id`),
    * le titre du cycle de vie (`title`),
* `tags` : tag documentaire associés au document, cette structure est une liste de valeurs,
* `security` : information liées aux droits du documents, cette structure contient 
    * `lock` : verrou. La propriété contient : 
        * `id` identifiant du verrou, 
        * `temporary` :  indique si le verrou est temporaire ou permanent,
        * `lockedBy` : identifiant du compte de l'utilisateur ayant verrouillé le document, 
    * `readOnly` : un booléen indiquant si le document est en lecture seule , 
    * `fixed` :un booléen indiquant si le document est figé , 
    * `profil` : profil associé au document  : 
         * `id` :  identifiant du profil, 
         * `icon` :  l'adresse de téléchargement de l'icône du profil , 
         * `type` : type  : `linked` (lié) ou `private` (privé), 
         * `activated` : un booléen indiquant si le profil est activé , 
         * `title` : titre du profil,
    * `confidentiality` : sa confidentialité, 
* `status` : information sur le statut du document, trois valeurs sont possibles 
    * `deleted` : document supprimé,
    * `fixed` : document figé,
    * `alive` : document vivant,
* `note` : liste des post-it associés au document,
* `type` : type du document, les valeurs suivantes sont possibles :
    * `document` : document standard,
    * `folder` : dossier : collection statique de documents,
    * `search` : recherche : collection dynamique de documents,
    * `family` : famille : définition d'une famille de documents,
    * `profil` : profil : document de modèle de profil,
    * `workflow` : document de cycle de vie,
* `state` : état du document, cette structure contient :
    *   `reference` : clef de l'étape,   
    *   `color` : couleur de l'étape,   
    *   `activity` : activité de l'étape (vide si document est figé, - ancienne révision - ),   
    *   `stateLabel` : libellé de l'état de l'étape,   
    *   `displayValue` : libellé de l'étape. Il est égal à l'activité si le 
      document est en dernière révision et si l'activité est déclarée sinon il 
      est égal au libellé de l'état,
* `usage` : Usage du document. Les valeurs suivantes sont possibles :
    *   `normal` : usage normal,   
    *   `system` : document de configuration (exemple cycle de vie, profil).

Exemple :

    [php]
    $searchDoc=new SearchDoc('',"ZOO_TEST");
    $searchDoc->setObjectReturn();
    $documentList=$searchDoc->getDocumentList();
    
    $formatCollection=new FormatCollection();
    $formatCollection->useCollection($documentList);
    $formatCollection->addProperty($formatCollection::propState);
    $r=$formatCollection->render();
    print_r($r);

Le résultat est :

    [php]
    ["properties"] => Array 
                ( 
                    ["id"] => 67978 
                    ["title"] => "Nouga" 
                    ["state"] => statePropertyValue Object 
                        ( 
                            ["reference"] => "zoo_transmited" 
                            ["color"] => "#A8DF78" 
                            ["activity"] => "Vérification de l'adoption" 
                            ["stateLabel"] => "Transmis" 
                            ["displayValue"] => "Vérification de l'adoption" 
                        ) 
                ) 
                

Un exemple de rendu de l'ensemble des propriétés sérialisé en JSON :

    [javascript]
    {
        "id": 1289,
        "title": "2013-05-07",
        "adate": "2014-12-16 16:51:42",
        "affected": {
            "id": 0,
            "title": ""
        },
        "allocated": null,
        "atags": null,
        "cdate": "2014-12-16 16:51:42",
        "confidential": null,
        "createdBy": {
            "id": 1008,
            "title": "Master Default",
            "icon": "resizeimg.php?img=Images%2Fdynacase-iuser.png&size=24"
        },
        "creationDate": "2014-12-16 16:51:42",
        "cvid": null,
        "doctype": "F",
        "dprofid": "0",
        "family": {
            "title": "Entrée",
            "name": "ZOO_ENTREE",
            "id": 1059,
            "icon": "resizeimg.php?img=Images%2Fentree.gif&size=24"
        },
        "fromid": "1059",
        "icon": "resizeimg.php?img=Images%2Fentree.gif&size=24",
        "initid": 1289,
        "lastAccessDate": "2014-12-16 16:51:42",
        "lastModificationDate": "2014-12-16 16:52:06",
        "lmodify": "Y",
        "locked": 0,
        "name": null,
        "note": {
            "id": 0,
            "title": ""
        },
        "owner": "1",
        "postitid": null,
        "prelid": null,
        "profid": "1084",
        "revdate": "2014-12-16 16:52:06",
        "revision": 0,
        "revisionData": {
            "isModified": true,
            "id": 1289,
            "number": 0,
            "createdBy": {
                "id": 1008,
                "title": "Master Default",
                "icon": "resizeimg.php?img=Images%2Fdynacase-iuser.png&size=24"
            }
        },
        "security": {
            "lock": {
                "id": 0
            },
            "readOnly": false,
            "fixed": false,
            "profil": {
                "id": 1084,
                "icon": "resizeimg.php?img=Images%2Fprofil.gif&size=24",
                "type": "linked",
                "activated": true,
                "title": "Caisse"
            },
            "confidentiality": "public"
        },
        "status": "alive",
        "tags": [],
        "type": "document",
        "usage": "normal",
        "usefor": "N",
        "version": null,
        "viewController": {
            "id": 0,
            "title": ""
        },
        "wid": null,
        "workflow": {
            "id": 0,
            "title": ""
        }
    }

## Formatage des attributs {#core-ref:683390ec-6b17-4ade-be6e-6085f4893f7f}

Les valeurs des attributs se placent dans le tableau "attributes". Chacun des
attributs a un champ "value" qui indique la valeur brute et "displayValue" qui
indique la valeur affichable.  Pour ajouter les attributs à formater, il faut
utiliser la méthode "addAttribute".

    [php]
    $formatCollection=new FormatCollection();
    $formatCollection->useCollection($documentList);
    $formatCollection->addAttribute("tst_title")->addAttribute("tst_double");
    $r=$formatCollection->render();

Extrait de la partie "attributes"

    [php]
    [attributes] => Array
                (
                    ["tst_title"] => textAttributeValue Object
                        (
                            ["value"] => "Le tour du monde"
                            ["displayValue"] => "Le tour du monde"
                        )
                    ["tst_double"] => doubleAttributeValue Object
                        (
                            ["value"] => 3.14
                            ["displayValue"] => "3,14"
                        )

<span class="flag inline nota-bene"/> : Si un attribut n'a pas de valeur, le formatage
sera "null" quelque soit le type d'attribut.  

Si un attribut n'existe pas la structure (value, displayValue) sera remplie avec
la variable identifiée par la méthode "::setNc()".

    [php]
    $searchDoc=new SearchDoc('',"DIR");
    $searchDoc->setObjectReturn();
    $documentList=$searchDoc->search()->getDocumentList();
    $formatCollection=new FormatCollection();
    $formatCollection->useCollection($documentList);
    $formatCollection->setNc("nc");
    $formatCollection->addProperty($formatCollection::propState);
    $formatCollection->addAttribute("tst_title")->addAttribute("ba_desc");
    $r=$formatCollection->render();


Exemple avec un attribut "vide" (ba_desc) et un attribut inexistant.

    [php]
    [attributes] => Array 
                    ( 
                        ["tst_title"] => unknowAttributeValue Object 
                            ( 
                                ["value"] => "nc" 
                                ["displayValue"] => "nc" 
                            ) 
                        ["ba_desc"] => null
                    ) 

Si l'option "`showempty`" est indiquée dans l'attribut, le rendu d'une valeur
nulle sera remplacée par la valeur de l'option.

## Rendu des attributs {#core-ref:f62199d8-8388-48a2-b79a-327307b58a86}

### type `text` {#core-ref:5de7150f-c887-481d-95d4-51a54df7363a}

    [php]
    ["tst_text"] => textAttributeValue Object
                    (
                        ["value"] => "Testing"
                        ["displayValue"] => "before Testing"
                    )

La valeur formatée tient compte du format mis dans le type  dans cet exemple :
text("before %s")


### type `int` {#core-ref:4f6dcf5c-f929-429e-8158-94aedec2eaa4}

    [php]
    [tst_int] => intAttributeValue Object
                        (
                            ["value"] => 10
                            ["displayValue"] => "0010"
                        )
                        
La valeur formatée tient compte du format mis dans le type.
Dans cet exemple : int("%04d"). La valeur est de type "int".

### type `double` {#core-ref:84b38e2a-d231-4fc3-9ccc-e78b8b90a949}

    [php]
    [tst_double] => doubleAttributeValue Object
                        (
                            ["value"] => 0
                            ["displayValue"] => "0,00"
                        )

La valeur formatée tient compte du format mis dans le type. Dans cet exemple :
double("%.02f"). Le point est transformé en virgule si la locale est "fr\_FR".
La valeur est de type "double".


### type `date` {#core-ref:3f8e0b89-db33-4be8-b68e-15f709ecd0a8}

    [php]
    [tst_date] => dateAttributeValue Object
                        (
                            ["value"] => "2012-06-08"
                            ["displayValue"] => "08/06/2012"
                        )

La valeur formatée tient compte du format mis dans le type. S'il n'y a pas de
format, cela dépend du format de la locale de l'utilisateur.

### type `timestamp` {#core-ref:4bdcbdc6-018a-456c-9747-96b3825f6275}

    [php]
    [tst_ts] => dateAttributeValue Object
                        (
                            ["value"] => "2012-06-13 11:27:00"
                            ["displayValue"] => "13/06/2012 11:27"
                        )

La valeur formatée tient compte du format mis dans le type. S'il n'y a pas de
format, cela dépend du format de la locale de l'utilisateur.

### type `file` {#core-ref:732f5207-8e22-4e21-86d4-f8023fdd2706}

    [php]
    ["tst_file"] => fileAttributeValue Object
        (
            ["size"] => 572
            ["creationDate"] => "2014-10-14 11:44:20"
            ["fileName"] => "iuserplus.csv"
            ["url"] => "file/13502/807/fi_ofile/1/iuserplus.csv?cache=no&inline=yes"
            ["mime"] => "text/plain"
            ["icon"] => "resizeimg.php?img=CORE%2FImages%2Fmime-txt.png&size=14"
            ["value"] => "text/plain|807|iuserplus.csv"
            ["displayValue"] => "iuserplus.csv"
        )

La valeur formatée est le titre du fichier.

Éléments retournés :

*   `size` : taille en octets du fichier
*   `creationDate` : date d'enregistrement du fichier
*   `fileName` : nom du fichier
*   `url` : url de téléchargement du fichier
*   `mime` : type mime du fichier
*   `icon` : icône représentant le type de fichier
*   `value` : valeur brute (référence au fichier)
*   `displayValue` : nom du fichier

### type `image` {#core-ref:483885aa-4f79-4dfb-821c-d991f435f956}

    [php]
    ["tst_img"] => imageAttributeValue Object
    (
        ["thumbnail"] => "file/13505/810/img_file/-1/tree.jpg?cache=no&inline=yes&width=48"
        ["size"] => 584894
        ["creationDate"] => "2014-10-14 13:59:30"
        ["fileName"] => "tree.jpg"
        ["url"] => "file/13505/810/img_file/-1/tree.jpg?cache=no&inline=yes"
        ["mime"] => "image/jpeg"
        ["icon"] => "resizeimg.php?img=CORE%2FImages%2Fmime-image.png&size=24"
        ["value"] => "image/jpeg|810|tree.jpg"
        ["displayValue"] => "tree.jpg"
     )


Éléments retournés :

*   `thumbnail` : url de téléchargement de la miniature de l'image
*   `size` : taille en octets de l'image
*   `creationDate` : date d'enregistrement de l'image
*   `fileName` : nom de l'image
*   `url` : url de téléchargement de l'image
*   `mime` : type mime de l'image
*   `icon` : icône représentant le type de l'image
*   `value` : valeur brute (référence à l'image)
*   `displayValue` : nom de l'image

Note : La largeur de la miniature peut être définie avec l'attribut
`imageThumbnailSize` de la classe `FormatCollection`. Elle est de 48px par défaut.

### type `docid` - `account` {#core-ref:95aee029-f729-41f9-9b18-a6c20813c24d}

    [php]
    ["tst_relation"] => docidAttributeValue Object
    (
        ["familyRelation"] => "TST_FMTCOL"
        ["url"] => "?app=FDL&amp;OPENDOC&amp;mode=view&amp;id=84412&amp;latest=Y"
        ["icon"] => "resizeimg.php?img=Images/test.png&size=14"
        ["revision"] => -1
        ["initid"] => 84412
        ["value"] => "84412"
        ["displayValue"] => "Test 1"
    )

* `familyRelation` : indique le format du type de relation.
* `url` : L'url permet d'accéder à la page de consultation du document. 
* `icon` : La taille de l'icône du document pointé est par défaut de 14px. Elle
peut être modifiée avec l'attribut `relationIconSize` de la classe
`FormatCollection`.
* `revision` : <span class="flag from release">3.2.21</span> Contient 
    le numéro de la révision si l'option "[`docrev`][docrev]`=fixed`".  
    Contient "`state:<état>`" si l'option est "[`docrev`][docrev]`=state(<état>)`".  
    Sinon contient `-1` pour indiquer que c'est la dernière révision qui doit 
    être utilisée.
*  `initid` : <span class="flag from release">3.2.21</span>La propriété
    [`initid`][properties] du document cible
* `value` : La valeur brute indique l'identifiant numérique du document stocké.
    Il est généralement différent de l'`initid` en cas de lien vers une révision
    spécifique. 
* `displayValue` : La valeur formatée indique le titre du document pointé. 

### type `enum` {#core-ref:ddcd138f-acbd-46f0-bf82-2227399536dc}

    [php]
    ["tst_enum"] => enumAttributeValue Object
    (
        ["value"] => 1
        ["displayValue"] => "Un"
        ["exists"] => true
    )

La valeur formatée donne le libellé de l'énuméré. 

<span class="flag from release inline">3.2.21</span> La propriété "exists" indique
si la valeur fait partie du référenciel de l'énuméré. Cette propriété peut être
`false` dans le cas de l'option [`etype=free`][etype] : autorisation de pose 
d'un autre choix par l'utilisateur.

### type `htmltext` {#core-ref:8564f4bc-d65d-43b4-8908-b510598e2a3c}

Si le flag `stripHtmlTags` (méthode `stripHtmlTags`) est à `true` les balises
HTML sont supprimées dans la `displayValue`.

### type `longtext` {#core-ref:0dca5896-2598-4f7c-bb81-1cf4a82c7024}

<span class="flag from release">3.2.18</span> Si l'attribut est dans un
array les séparateurs de cellules sont des sauts de lignes `\n`). Vous pouvez
les configurer à l'aide de la méthode `setLongtextMultipleBrToCr`.

### Autres types {#core-ref:45f81665-a96a-406b-8722-a161340b15eb}

    [php]
    ["x_attr"] => standardAttributeValue Object
    (
        ["value"] => "12:20:00"
        ["displayValue"] => "12:20:00"
    )

La valeur et la valeur formatée sont égales.

## Rendu des attributs multiples {#core-ref:2256843a-e63f-484b-a00d-c2a85253deea}

Les attributs multiples sont rendus dans des tableaux de structure.
Exemple avec deux valeurs de l'attribut "tst\_colors".

    [php]
    ["tst_colors"] => Array
    (
        [0] => standardAttributeValue Object
            (
                ["value"] => "#80CCFF"
                ["displayValue"] => "#80CCFF"
            )
        [1] => standardAttributeValue Object
            (
                ["value"] => "#FFD77A"
                ["displayValue"] => "#FFD77A"
            )
    )

La même structure à deux niveaux est rendue pour les multiples dans les
tableaux. Cela est limité à l'attribut docid.

    [php]
    [tst_relation_multiple] => Array
    (
       [0] => Array
           (
               [0] => docidAttributeValue Object
                   (
                       ["familyRelation"] => "TST_FMTCOL"
                       ["url"] => "?app=FDL&amp;action=OPENDOC..."
                       ["icon"] => 
                       ["initid"] => 84412
                       ["revision"] => -1
                       ["value"] => "84412"
                       ["displayValue"] => "Test 1"
                   )
           )
       [1] => Array
           (
               [0] => docidAttributeValue Object
                   (
                       ["familyRelation"] => "TST_FMTCOL"
                       ["url"] => ?"app=FDL&amp;action=OPENDOC..."
                       ["icon"] => 
                       ["initid"] => 84412
                       ["revision"] => -1
                       ["value"] => "84412"
                       ["displayValue"] => "Test 1"
                   )
               [1] => docidAttributeValue Object
                   (
                       ["familyRelation"] => "TST_FMTCOL"
                       ["url"] => "?app=FDL&amp;action=OPENDOC..."
                       ["icon"] => 
                       ["initid"] => 86854
                       ["revision"] => -1
                       ["value"] => "86854"
                       ["displayValue"] => "Test 2"
                   )

## Hook de rendu {#core-ref:d25bfead-3edf-4d35-abff-f4892b0237f2}

<span class="flag from release">3.2.18</span> Le rendu proposé par la  classe
n'est pas toujours le rendu attendu par un outil externe, il est possible de
modifier celui-ci en utilisant les hook de rendu.

### Hook de document {#core-ref:9a0d523c-bbd2-45d4-beb0-7d06d5a7a41f}

Il est possible d'inscrire un hook qui est appliqué après le rendu de chaque
document pour modifier le tableau formaté produit.

Exemple, ajout d'une URI particulière :

    [php]
    $formatCollection->setDocumentRenderHook(function ($values, \Doc $document) {
        $values["uri"] = sprintf("http://api.example.net/%s", $document->getPropertyValue("initid");
        return $values;
    });

Le hook s'ajoute à l'aide de la fonction `setDocumentRenderHook` celle-ci prend
en entrée un callable et l'exécute à chaque rendu de document. La fonction
reçoit en entrée :

* values : le tableau de valeur calculé,
* document : l'objet document en cours.

Elle doit retourner l'objet values modifié.

### Hook d'attribut {#core-ref:731be72d-8d27-4da3-a969-715347db4b7f}

Il est possible d'inscrire un hook qui est appliqué après le rendu de chaque
attribut pour modifier le rendu d'attribut produit.

Exemple, uniformisation des valeurs vides :

    [php]
    $formatCollection->setAttributeRenderHook(function ($attributeValue, $attribute) {
         if ($attributeValue === null) {
             /**
              * @var \NormalAttribute $attribute
              */
             if ($attribute->isMultiple()) {
                 $attributeValue = array();
             } else {
                 $attributeValue = new \StandardAttributeValue($attribute, null);
             }
         }
         return $attributeValue;
     });

Le hook s'ajoute à l'aide de la fonction `setAttributeRenderHook` celle-ci prend
en entrée un callable et l'exécute à chaque rendu d'attribut. La fonction reçoit
en entrée :

* attributeValue : la valeur de l'attribut calculé,
* attribute : l'objet attribut en cours,
* document : l'objet document en cours.

Elle doit retourner l'objet attributeValue modifié.

### Hook de propriété {#core-ref:ef73ccba-b9f2-48fd-8d37-dcb75a4cc7e2}

Il est possible d'inscrire un hook qui est appliqué après le rendu de chaque
propriété pour modifier le rendu de la propriété.

Exemple, passage à `false` de la propriété `state` pour les documents n'ayant
pas d'état :

    [php]
    $formatCollection->setPropertyRenderHook(function ($propertyValue, $propertyId) {
         if ($propertyId === "state" && !$propertyValue->reference) {
             $propertyValue = false;
         }
         return $propertyValue;
     });

Le hook s'ajoute à l'aide de la fonction `setPropertyRenderHook` celle-ci prend
en entrée un callable et l'exécute à chaque rendu de propriété. La fonction
reçoit en entrée :

* propertyValue : la valeur de la propriété calculée,
* propertyId : identifiant de la propriété en cours,
* document : l'objet document en cours.

Elle doit retourner l'objet propertyValue modifié.

<!-- links -->

[ClassDocumentList]: #core-ref:23c71c28-dbce-4d34-819a-50d5bc4a38c3
[ClassSearchDoc]: #core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255
[intro_collection]: #core-ref:a2c4bd53-c31f-4448-82e4-7ec1d2f7f346
[propriete_doc]: #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353
[visibility]:   #core-ref:3e67d45e-1fed-446d-82b5-ba941addc7e8
[docrev]:       #core-ref:9bcfd205-fb07-4a71-be06-ba07d4a9cc7c
[properties]:   #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353
[etype]:        #core-ref:4f14142f-6aac-473f-a0e3-87803febb883