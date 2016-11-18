# Déclaration de familles {#core-ref:cfc7f53b-7982-431e-a04b-7b54eddf4a75}

## Introduction {#core-ref:4b4cb93e-e717-4a42-888d-c2376deab4bb}

Pour ajouter des familles de document à Dynacase, on va utiliser divers formats
de fichier :

*   Les familles seront définies dans des fichiers csv ;
*   Le code php sera à déployer dans des fichiers php, à intégrer à la plate-forme ;
*   Les documents systèmes seront importés au moyen de fichiers csv ou xml.

## Définition de familles {#core-ref:17500007-32d8-4aee-bc3f-7e569e1cd5a6}

Les familles sont définies dans un fichier csv respectant le format suivant :

*   Encodage : UTF-8,
*   Délimiteur de texte : ` ` (vide),
*   séparateur de colonnes : `;`.

Exemple de définition d'une famille :

    BEGIN;;Animal;;;ZOO_ANIMAL;;;;;;;;;;;
    //;properties;;;;;;;;;;;;;;;
    //propid;value;;;;;;;;;;;;;;;
    ICON;zoo_animal.png;;;;
    CLASS;Zoo\Zoo_animal;;;;
    DFLDID;FLD_ZOO_ANIMAL;;;;
    ;;;;;;;;;;;;;;;;
    //;attributes;;;;;;;;;;;;;;;
    //;idattr;idframe;label;T;A;type;ord;vis;need;link;phpfile;phpfunc;elink;constraint;option;Commentaires
    ;;;;;;;;;;;;;;;;
    ATTR;AN_IDENTIFICATION;;Identification;N;N;frame;::auto;W;;;;;;;;
    ATTR;AN_NOM;AN_IDENTIFICATION;nom;Y;N;text;::auto;W;Y;;;;;;edittemplate=ZOO:ANIMALNAME:U|viewtemplate=ZOO:ANIMALNAME;
    ATTR;AN_TATOUAGE;AN_IDENTIFICATION;tatouage;N;N;int;::auto;W;;;;;;;edittemplate=ZOO:ANIMALTATOO:S|viewtemplate=ZOO:ANIMALTATOO:S;
    ATTR;AN_ESPECE;AN_IDENTIFICATION;espèce;N;N;docid("ZOO_ESPECE");::auto;W;Y;;;;;;creation={es_nom:CT}|doctitle=an_espece_title;
    ATTR;AN_ESPECE_TITLE;AN_IDENTIFICATION;espèce (titre);Y;N;text;::auto;H;;;;::getTitle(an_espece);;;;
    ATTR;AN_ORDRE;AN_IDENTIFICATION;ordre;N;N;text;::auto;R;;;;::getdocvalue(an_espece,es_ordre);;;;
    ATTR;AN_CLASSE;AN_IDENTIFICATION;classe;N;N;docid("ZOO_CLASSE");::auto;R;;;;::getdocvalue( an_espece , es_classe);;;doctitle=auto;
    ATTR;AN_SEXE;AN_IDENTIFICATION;sexe;N;N;enum;::auto;W;;;;M|Masculin,F|Féminin,H|Hermaphrodite;;;;
    ATTR;AN_PHOTO;AN_IDENTIFICATION;photo;N;N;image;::auto;W;;;;;;;;
    ATTR;AN_NAISSANCE;AN_IDENTIFICATION;date naissance;N;N;date;::auto;W;;;;;;::validatePastDate(AN_NAISSANCE);;
    ATTR;AN_ENTREE;AN_IDENTIFICATION;date entree;N;N;date;::auto;W;;;;;;::validatePastDate(AN_ENTREE);;
    ATTR;AN_ENFANT_T;AN_IDENTIFICATION;liste enfant;N;N;array;::auto;W;;;;;;;;
    ATTR;AN_ENFANT;AN_ENFANT_T;enfant;N;N;docid("ZOO_ANIMAL");::auto;W;;;;;;;creation={an_nom:CT,an_espece:an_espece};
    ATTR;AN_CARNETSANTE;AN_IDENTIFICATION;Carnet Santé;N;N;menu;::auto;W;;%S%app=GENERIC&action=GENERIC_ISEARCH&id=%I%&famid=ZOO_CARNETSANTE&viewone=Y;;;;;;
    ATTR;AN_ENCLOS;AN_IDENTIFICATION;Enclos;N;N;menu;::auto;W;;%S%app=GENERIC&action=GENERIC_ISEARCH&id=%I%&famid=ZOO_ENCLOS&viewone=Y;;;;;;
    ATTR;AN_PARENT;AN_IDENTIFICATION;Parents;N;N;menu;::auto;W;;%S%app=GENERIC&action=GENERIC_ISEARCH&generic=Y&id=%I%&famid=ZOO_ANIMAL;;;;;;
    ATTR;AN_PERE;AN_IDENTIFICATION;pere;N;Y;docid("ZOO_ANIMAL");::auto;R;;;;::getAscendant(M);;;doctitle=auto
    ATTR;AN_MERE;AN_IDENTIFICATION;mere;N;Y;docid("ZOO_ANIMAL");::auto;R;;;;::getAscendant(F);;;doctitle=auto
    ATTR;AN_FOLDER;;Dossier;N;N;menu;::auto;W;;%S%app=ZOO&action=ZOO_ANIMALFOLDER&id=%I%;;;;;
    ;;;;;;;;;;;;;;;;
    END;;;;;;;;;;;;;;;;

**Note** : Le format `ODS` (openDocument Spread Sheet) peut aussi être utilisé
comme format de fichier d'importation de famille ou de document.

Ce qui donne, vu dans un tableau :

|          |                   |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| -        |                   |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| BEGIN    |                   | Animal            |                |   | ZOO_ANIMAL |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| //       | properties        |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| //propid | value             |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| ICON     | zoo_animal.png    |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| CLASS    | Zoo\Zoo_animal    |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| DFLDID   | FLD_ZOO_ANIMAL    |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
|          |                   |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| //       | attributes        |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| //       | idattr            | idframe           | label          | T | A          | type                | ord    | vis | need | link                                                                         | phpfile | phpfunc                                             | elink | constraint                       | option                                   | Commentaires                   |   |   |
|          |                   |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| ATTR     | AN_IDENTIFICATION |                   | Identification | N | N          | frame               | ::auto | W   |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| ATTR     | AN_NOM            | AN_IDENTIFICATION | nom            | Y | N          | text                | ::auto | W   | Y    |                                                                              |         |                                                     |       |                                  | edittemplate=ZOO:ANIMALNAME:U            | viewtemplate=ZOO:ANIMALNAME    |   |   |
| ATTR     | AN_TATOUAGE       | AN_IDENTIFICATION | tatouage       | N | N          | int                 | ::auto | W   |      |                                                                              |         |                                                     |       |                                  | edittemplate=ZOO:ANIMALTATOO:S           | viewtemplate=ZOO:ANIMALTATOO:S |   |   |
| ATTR     | AN_ESPECE         | AN_IDENTIFICATION | espèce         | N | N          | docid("ZOO_ESPECE") | ::auto | W   | Y    |                                                                              |         |                                                     |       |                                  | creation={es_nom:CT}                     | doctitle=an_espece_title       |   |   |
| ATTR     | AN_ESPECE_TITLE   | AN_IDENTIFICATION | espèce (titre) | Y | N          | text                | ::auto | H   |      |                                                                              |         | ::getTitle(an_espece)                               |       |                                  |                                          |                                |   |   |
| ATTR     | AN_ORDRE          | AN_IDENTIFICATION | ordre          | N | N          | text                | ::auto | R   |      |                                                                              |         | ::getdocvalue(an_espece,es_ordre)                   |       |                                  |                                          |                                |   |   |
| ATTR     | AN_CLASSE         | AN_IDENTIFICATION | classe         | N | N          | docid("ZOO_CLASSE") | ::auto | R   |      |                                                                              |         | ::getdocvalue( an_espece , es_classe)               |       |                                  | doctitle=auto                            |                                |   |   |
| ATTR     | AN_SEXE           | AN_IDENTIFICATION | sexe           | N | N          | enum                | ::auto | W   |      |                                                                              |         | M&#124;Masculin,F&#124;Féminin,H&#124;Hermaphrodite |       |                                  |                                          |                                |   |   |
| ATTR     | AN_PHOTO          | AN_IDENTIFICATION | photo          | N | N          | image               | ::auto | W   |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| ATTR     | AN_NAISSANCE      | AN_IDENTIFICATION | date naissance | N | N          | date                | ::auto | W   |      |                                                                              |         |                                                     |       | ::validatePastDate(AN_NAISSANCE) |                                          |                                |   |   |
| ATTR     | AN_ENTREE         | AN_IDENTIFICATION | date entree    | N | N          | date                | ::auto | W   |      |                                                                              |         |                                                     |       | ::validatePastDate(AN_ENTREE)    |                                          |                                |   |   |
| ATTR     | AN_ENFANT_T       | AN_IDENTIFICATION | liste enfant   | N | N          | array               | ::auto | W   |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| ATTR     | AN_ENFANT         | AN_ENFANT_T       | enfant         | N | N          | docid("ZOO_ANIMAL") | ::auto | W   |      |                                                                              |         |                                                     |       |                                  | creation={an_nom:CT,an_espece:an_espece} |                                |   |   |
| ATTR     | AN_CARNETSANTE    | AN_IDENTIFICATION | Carnet Santé   | N | N          | menu                | ::auto | W   |      | %S%app=GENERIC&action=GENERIC_ISEARCH&id=%I%&famid=ZOO_CARNETSANTE&viewone=Y |         |                                                     |       |                                  |                                          |                                |   |   |
| ATTR     | AN_ENCLOS         | AN_IDENTIFICATION | Enclos         | N | N          | menu                | ::auto | W   |      | %S%app=GENERIC&action=GENERIC_ISEARCH&id=%I%&famid=ZOO_ENCLOS&viewone=Y      |         |                                                     |       |                                  |                                          |                                |   |   |
| ATTR     | AN_PARENT         | AN_IDENTIFICATION | Parents        | N | N          | menu                | ::auto | W   |      | %S%app=GENERIC&action=GENERIC_ISEARCH&generic=Y&id=%I%&famid=ZOO_ANIMAL      |         |                                                     |       |                                  |                                          |                                |   |   |
| ATTR     | AN_PERE           | AN_IDENTIFICATION | pere           | N | Y          | docid("ZOO_ANIMAL") | ::auto | R   |      |                                                                              |         | ::getAscendant(M)                                   |       |                                  | doctitle=auto                            |                                |   |   |
| ATTR     | AN_MERE           | AN_IDENTIFICATION | mere           | N | Y          | docid("ZOO_ANIMAL") | ::auto | R   |      |                                                                              |         | ::getAscendant(F)                                   |       |                                  | doctitle=auto                            |                                |   |   |
| ATTR     | AN_FOLDER         |                   | Dossier        | N | N          | menu                | ::auto | W   |      | %S%app=ZOO&action=ZOO_ANIMALFOLDER&id=%I%                                    |         |                                                     |       |                                  |                                          |                                |   |   |
|          |                   |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |
| END      |                   |                   |                |   |            |                     |        |     |      |                                                                              |         |                                                     |       |                                  |                                          |                                |   |   |


La définition d'une famille commence toujours par une ligne de la forme :

    BEGIN;[fromid];[title];[id];[className];[logicalName]

et se termine toujours par une ligne de la forme :

    END;

avec les correspondances suivantes:

BEGIN
:   Obligatoire, signale le début d'une définition de famille.

[fromid]
:   Identifiant logique (nom logique ou identifiant interne numérique) de la
 famille de laquelle cette famille hérite.
    
    Laisser vide s'il n'y a pas d'héritage.
    
    L'utilisation d'un identifiant interne numérique à la place d'un nom
    logique est à réserver aux familles de core.

[title]
:   Titre de la famille.
    
    Ce titre est utilisé sur les IHM pour désigner la famille.
    
    Il est automatiquement ajouté au catalogue de traduction, et peut ainsi
    être traduit.

[id]
:   Identifiant numérique de la famille.
    
    Laisser vide pour utiliser un identifiant logique
    (Dans ce cas, Dynacase affectera automatiquement un identifiant interne
     unique à la famille).
    
    S'il est renseigné, il faut que cet identifiant ne soit pas déjà pris par
     un autre document.
    
    Les valeurs entre 900 et 999 peuvent être utilisée pour vos besoins
    spécifiques, bien que l'usage de valeurs numériques fixes soit fortement
    déconseillée.

[className] (déprécié)
:   Nom de la classe PHP utilisée pour cette famille.
    
    Cette classe doit être présente sur le serveur dans un fichier appelé
    `/FDL/Class.[CLASSNAME].php`.
    
    Permet un héritage autre que celui prévu par défaut par les classes
    documentaires.
    
    L'utilisation de la propriété `CLASS` permet de réaliser cette 
    fonctionnalité.
    
    Pour supprimer cette propriété, il faut mettre deux tirets `--` comme valeur.
    Si la valeur est vide, la propriété conserve son ancienne valeur.

[logicalName]
:   Nom logique de la famille.
    
    Doit commencer par une lettre.
    Il ne peut ensuite contenir que des caractères alphanumériques ainsi
    que les caractères _ et - (pas d'espace, ni de ponctuation).

END
:   Obligatoire, signale la fin d'une définition de famille.

Entre ces 2 lignes, chacune des lignes correspond à :

*   un [paramètre de propriété](#core-ref:40d229c4-33c4-11e2-9147-a3eaf356c37c),
*   une [propriété de famille](#core-ref:6f013eb8-33c7-11e2-be43-373b9514dea3),
*   un [attribut](#core-ref:bc3fad86-33cc-11e2-9a69-1bbd9c32b0f2),
*   un [paramètre de famille](#core-ref:c28824e2-3486-11e2-be3b-337d2321d8ee),
*   une [valeur par défaut](#core-ref:94fa51e2-3488-11e2-9e34-1f7c912168cf),
*   une [valeur initiale de paramètre](#core-ref:da804e2e-3573-11e2-8974-4ba96567fbf9),
*   une [modification d'attribut père](#core-ref:13dcd96d-561f-463c-a88f-4b9db58e4fbd),
*   une [instruction de réinitialisation](#core-ref:5c661733-772d-42b8-8b3e-b70453ddfd33),
*   un commentaire.
    
    Une ligne qui ne sera pas traitée lors du traitement de la définition de famille.
    
    Un commentaire commence toujours par `//`. Par exemple : `// Ceci est un commentaire;;;;;;;;;;;;;;;;`

### Définition de paramètres de propriété {#core-ref:40d229c4-33c4-11e2-9147-a3eaf356c37c}

Des paramètres permettent de modifier le comportement des propriétés du document.

Leur syntaxe est toujours de la forme `PROP;[propid];[param]`,
avec les correspondances suivantes :

[propid]
:   Nom de la propriété.

[param]
:   Définition du paramètre, sous la forme `[parameterName]=[parameterValue]`.

Par exemple :

    PROP;title;sort=asc

Les paramètres de propriété disponibles sont :

sort
:   Permet de spécifier si la propriété est disponible dans les recherches
    et les rapports, et quel est son ordre de tri par défaut.
    
    Les valeurs possibles sont :
    
    *   `no` : la propriété n'apparaîtra pas dans les recherches et rapports ;
    *   `asc` : la propriété est disponible dans les recherches et les rapports ;
        et est trié par défaut par ordre ascendant ;
    *   `desc` : la propriété est disponible dans les recherches et les rapports ;
        et est trié par défaut par ordre descendant.
    
    Les propriétés suivantes ont un paramètre *sort* par défaut :
    
    *   initid : sort=desc,
    *   revdate : sort=desc,
    *   state : sort=asc,
    *   title : sort=asc
    
    Les autres propriétés sont par défaut à *sort=no*.

### Définition de propriétés de famille {#core-ref:6f013eb8-33c7-11e2-be43-373b9514dea3}

Les propriétés de famille permettent, selon les cas,
de définir un comportement particulier pour la famille,
ou de définir les valeurs par défaut des propriétés
des nouveaux documents de cette famille.

Leur syntaxe est toujours de la forme `[propid];[value]`, avec les
correspondances suivantes :

[propid]
:   Identifiant de la propriété.

[value]
:   Valeur de la propriété.

Les différents identifiants de propriété sont les suivants :

CPROFID
:   Indique le *profil* utilisé pour les documents créé avec cette famille.
    Affecte la valeur de la propriété `profid` pour les nouveaux documents de
    cette famille.  
    Contient l'identifiant d'un document profil.
    
    Lors de l'importation, les vérifications suivantes sont effectuées :
    
    *   Si la famille hérite de dossier,
        le profil référencé doit être de la famille profil de dossier ;
    *   Si la famille hérite de recherche,
        le profil référencé doit être de la famille profil de recherche, ;
    *   Sinon le profil référencé doit être de la famille profil de document.
    
    Si la valeur est vide le profil de document par défaut est enlevé.
    
    **Note** : Cette propriété ne modifie pas les profils des documents de cette
    famille qui sont déjà créés au moment de la mise à jour de la famille.

CVID
:   Indique le *contrôle de vue* qui sera associé aux documents créés avec cette
    famille. Affecte la valeur de la propriété `cvid` pour les nouveaux
    documents de cette famille.  
    Contient l'identifiant d'un document *contrôle de vue*. 
    
    Lors de l'importation, les vérifications suivantes sont effectuées :
    
    *   Ce document doit être de la famille *contrôle de vue*.
    *   Ce contrôle de vue doit être applicable à la famille en cours.
    
    Si la valeur est vide le contrôle de vue par défaut est enlevé.
    
    **Note** : Cette propriété ne modifie pas les contrôles de vue des 
    documents de cette famille qui sont déjà créés au moment de 
    la mise à jour de la famille.  

DFLDID
:   Identifiant du dossier principal permettant de constituer une arborescence
    spécifique à la famille .
    
    Ce dossier est nécessaire pour manipuler les documents d'une famille 
    depuis l'application "ONEFAM".
    Il peut être égal à `auto`, ce qui a pour effet de créer un dossier
    principal automatiquement. 
    Ce dossier aura les restrictions indiquant que seuls des documents de 
    cette famille et des dossiers pourront être insérés dans ce dossier 
    principal.
    
    Si cette propriété est déjà renseignée, deux cas de figure se présentent :
    
    *   La nouvelle valeur est 'auto' : Dans ce cas, l'ancienne valeur est
        conservée
    *   La nouvelle valeur n'est pas 'auto' : la nouvelle valeur est utilisée.
    
    Si la valeur est vide le dossier principal par défaut est enlevé.

ICON
:   Nom du fichier image définissant l'icône de la famille.
    
    Cette icône doit être une image carré. 
    La taille conseillée varie de 48 à 128 pixels. 
    Le format d'image conseillé est `png`. 
    Les formats d'images supportés sont `png` et `gif`.
    
    Si cette propriété est déjà renseignée, la nouvelle valeur ne sera pas
    prise en compte.
    
    Si la famille n'a pas encore d'icône, alors la nouvelle valeur est prise en
    compte.

CLASS
:   Indique le nom de la classe **_métier_** utilisée par la famille.
    
    Ce nom de classe doit être unique parmi l'ensemble des classes PHP utilisées
    sur le serveur. Il est recommandé d'utiliser un _namespace_ afin d'éviter un
    conflit de nom.
    
    Si la famille définie n'a pas de parent alors la classe _métier_ doit étendre la
    classe  `Dcp\Family\Document`. Cette classe est une classe héritant de la
    classe `Doc`.
    
    Exemple :
    
    | BEGIN |                   | Ma première famille |     | MY_FIRST |
    | ----- | ----------------- | ------------------- | --- | -------- |
    | CLASS | My\\MyFirstFamily |                     |     |          |
    | END   |                   |                     |     |          |
    
        [php]
        namespace My;
        class MyFirstFamily extends \Dcp\Family\Document {
            public function myFirstProcedure($x) {
                return $x+1;
            }
        }
    
    Si la classe est utilisée avec une famille héritant d'une autre famille,
    cette classe doit hériter de la classe générée de la famille parente.
    
    Exemple :
    
    | BEGIN |       IMAGE       |  Photographie |              | MY_PHOTO |     |          |        |
    | ----- | ----------------- | ------------- | ------------ | -------- | --- | -------- | ------ |
    | CLASS | My\\MyPhotoFamily |               |              |          |     |          |        |
    | ATTR  | MYPHO_FR_INFO     |               | Informations | N        | N   | frame    | ::auto |
    | ATTR  | MYPHO_EXIF        | MYPHO_FR_INFO | Exif         | N        | N   | longtext | ::auto |
    | END   |                   |               |              |          |     |          |        |
    
        [php]
        namespace My;
        class MyPhotoFamily extends \Dcp\Family\Image {
            public function postStore() {
                $err=parent::postStore();
                if (! $err) {
                    if (! $this->getRawValue(\Dcp\AttributeIdentifiers\Image::img_file)) {
                        $err=_("my::image needed");
                    }
                    if (! $this->getRawValue(\Dcp\AttributeIdentifiers\My\My_Photo::mypho_exif)) {
                        $err=_("my::no exif detected");
                    }
                }
                return $err;
            }
        }
    
    La classe peut étendre des classes intermédiaires, utiliser des interfaces
    ou des classes abstraites. La seule contrainte est que la classe générée de
    la famille parente doit faire partie de la hiérarchie de la classe _métier_.
    La classe générée de la famille hérite de cette classe. Elle apporte en plus
    la définition des attributs de la famille ainsi que le code généré pour les
    attributs calculés. Le nom de cette classe est `\Dcp\Family\<nom de la
    famille>`.
    
    Hiérarchie de classe lorsque la famille _MY_PHOTO_ est intégrée :
    
        [php]
        namespace Dcp\Core {
            // classe métier de la famille IMAGE
            class Images extends \Dcp\Family\Document {}
        }
        namespace Dcp\Family {
            // classe générée de la famille IMAGE
            class Images extends \Dcp\CoreFamily\Image {}
        }
        namespace My {
            // classe métier de la famille MY_PHOTO
            class MyPhotoFamily extends \Dcp\Family\Image {}
        }
        namespace Dcp\Family {
            // classe générée de la famille MY_PHOTO
            class My_photo extends \My\MyPhotoFamily {}
        }
        
    Les attributs des classes sont disponibles sous forme de constantes. Une
    classe d'attributs est générée lors de l'enregistrement de la famille. Cette
    classe est nommée avec le nom de la famille dans le namespace
    `\Dcp\AttributeIdentifiers`. L'usage des constantes permet de s'assurer de
    la validité des noms d'attributs. La classe d'attributs donne l'accès aux
    noms d'attribut de la famille et aussi à ceux de ses parents.
    
    Lors de l'importation de la famille les contraintes suivantes sont
    vérifiées :
    
    *   Le fichier PHP de la classe doit être accessible par l'_autoloader_.
        Les fichiers de classe sont généralement publiés dans le sous-répertoire
        de l'application livrée par le module.
        *   Le nom du fichier ne doit pas commencer par `Method`.
        *   L'extension du fichier doit être `php`.
    *   La classe doit hériter de la classe générée de la famille parente.
        En cas de famille sans héritage, la classe doit hériter de la classe
        `\Dcp\Family\Document`.
    *   La classe ne doit pas être abstraite.
    *   L'encodage du fichier doit être `utf-8` (incluant l'encodage `ascii`).
    *   Le fichier PHP doit être syntaxiquement correct.
    *   Le nom de la classe doit être unique parmi l'ensemble des classes php
        utilisées par Dynacase.
    *   La propriété `className` ne peut pas être utilisée conjointement avec la
        propriété `CLASS`.

METHOD
:   Cette propriété sert à réutiliser des morceaux de code entre plusieurs
    familles (Elle est à voir comme une version simplifiée des
    [traits][PHP_traits] pour les versions de php qui ne les supportent pas).
    
    Indique le nom du fichier PHP contenant les méthodes supplémentaires de la
    famille.
    
    Le fichier référencé doit être disponible dans le répertoire *FDL*. 
    
    **Note** : Le nom du fichier doit être unique parmi tous les fichiers
    présents dans le répertoire `FDL`. Il est fortement conseillé d'indiquer
    l'identifiant de la famille dans le nom de fichier
    (par exemple : `Method.MyFamily.php` où `MYFAMILY` est l'identifiant de la
    famille). Le nom du fichier doit commencer par `Method`.
    
    Cette propriété peut être utilisée plusieurs fois, avec la sémantique
    suivante :
    
    *   Lorsque le nom est préfixé par `+`, son contenu est concaténé
        directement dans la classe générée.
    *   Lorsque le nom est préfixé par `*`, le fichier n'est pas intégré
        directement dans la famille, mais une classe intermédiaire est générée.
        Cela permet notamment une surcharge plus fine des méthodes. Le prefix '*'
        ne peut être utilisé qu'une seule fois par famille.
    
    Si la valeur est vide, *toutes* les méthodes incluses au moyen de ce mot clé
    sont enlevées (y compris celles déclarées avec `*` ou `+`).
    
    Exemple :
    
    | BEGIN  |       IMAGE        |  Photographie |              | MY_PHOTO |     |          |        |
    | ------ | ------------------ | ------------- | ------------ | -------- | --- | -------- | ------ |
    | CLASS  | My\MyPhotoFamily   |               |              |          |     |          |        |
    | METHOD | Method.MyPhoto.php |               |              |          |     |          |        |
    | ATTR   | MYPHO_FR_INFO      |               | Informations | N        | N   | frame    | ::auto |
    | ATTR   | MYPHO_EXIF         | MYPHO_FR_INFO | Exif         | N        | N   | longtext | ::auto |
    | END    |                    |               |              |          |     |          |        |
    
    Hiérarchie de classe lorsque la famille _MY_PHOTO_ est intégrée :
    
        [php]
        namespace Dcp\Core {
            // classe métier de la famille IMAGE
            class Images extends \Dcp\Family\Document {}
        }
        namespace Dcp\Family {
            // classe générée de la famille IMAGE
            class Images extends \Dcp\CoreFamily\Image {}
        }
        namespace My {
            // classe métier de la famille MY_PHOTO
            class MyPhotoFamily extends \Dcp\Family\Image {}
        }
        namespace  {
            // classe méthode générée de la famille MY_PHOTO
            class _Method_MY_PHOTO_ extends \My\MyPhotoFamily {
                // inclus le contenu de Method.MyPhoto.php
            }
        }
        namespace Dcp\Family {
            // classe générée de la famille MY_PHOTO
            class My_photo extends _Method_MY_PHOTO_ {}
        }
    
    À la place de `METHOD`, l'utilisation de la propriété `CLASS` est
    recommandée afin de définir les classes _métier_ de la famille.

PROFID
:   Identifiant (nom logique ou identifiant interne) du document profil de
 famille pour cette  famille.
    
    Lors de l'importation, les vérifications suivantes sont effectuées :
    
    *   Ce document doit être de la famille 'profil de famille'.
    
    Si la valeur est vide le profil de famille est enlevé.

SCHAR
:   Indique les modalités de révision des documents de cette famille :
    
    *   *R* : Révision automatique à chaque modification,
    *   *S* : document non révisable
    
    Si la valeur est vide la caractéristique spéciale par défaut est enlevée.

TAG
:   Initialise les valeurs de la propriété *atags*.
    
    Chaque utilisation de la balise TAG ajoutera une valeur à la propriété
    *atags*.
    
    **Note** : Les tags applicatifs ne peuvent être supprimés par cette
    directive.
    
    Certains tags sont déjà prédéfinis par Dynacase :
    
    MAILRECIPIENT
    :   Déclare la famille comme *destinataire de mail*.
        Cela permet aux documents de cette famille d'être présentés dans la
        liste de destinataires lors des envois de mail.
        
        La famille doit alors implémenter l'interface [`IMailRecipient`][phpDocEmailRecipient].

TYPE
:   Valeur par défaut de la propriété *type*.
    
    Si la valeur est vide, le type inféré est *C*.

USEFOR
:   Caractère désignant une utilisation spéciale. Seulement pour les documents
    systèmes :
    
    *   *W* : pour les cycles de vie
    *   *G* : pour les intercalaires de chemise
    *   *P* : pour les profils
    
    Si la valeur est vide l'utilisation spéciale par défaut est enlevée.

WID
:   Indique le cycle de vie qui sera associé pour les documents créé avec 
cette famille. 
Affecte la valeur de la propriété `wid` pour les nouveaux documents de cette 
 famille. Contient l'identifiant d'un document *cycle de vie*. 
    
    Lors de l'importation, les vérifications suivantes sont effectuées :
    
    *   Ce document doit être un document [cycle de vie][workflow] ;
    *   Le cycle de vie doit être applicable à la famille en cours.
    
    Si la valeur est vide le cycle par défaut est enlevé.

### Définition d'attributs {#core-ref:bc3fad86-33cc-11e2-9a69-1bbd9c32b0f2}

Un attribut est défini par la syntaxe suivante :

    ATTR;[id_attribut];[id_conteneur];[label];[in_title];[in_abstract];[type];[ordre];[visibility];[required];[link];[phpfile];[phpfunc];[elink];[constraint];[options]

avec les correspondances suivantes :

#### ATTR {#core-ref:a7f05c53-688b-47f5-9ed2-4acf85545edf}

**Obligatoire**  
Signale que la ligne est une définition d'attribut.

#### Caractéristique `[id_attribut]` {#core-ref:0a4a238a-b3af-4879-b48b-2783ce5b9000}

**Obligatoire**  
Identifiant système de l'attribut.

Il sera automatiquement converti en minuscules.

Cet item n'est plus modifiable une fois créé car il peut être utilisé dans
les parties spécifiques de la famille.

Cet identificateur doit être unique dans la famille.

Il est conseillé d'adopter des règles de nommage des attributs, permettant
de simplifier leur manipulation.
Par exemple : `[FAM]_[TYPE]_[NAME]` avec :

*   [FAM] : un préfixe indiquant la famille,
*   [TYPE] : une lettre indiquant le type de l'attribut,
*   [NAME] : le nom de l'attribut.

#### Caractéristique `[id_conteneur]` {#core-ref:d273a1c6-2575-4671-a46f-f7f5b9473450}

**Obligatoire** (sauf pour les attributs de type *frame* ou *tab*)  
Identifiant système de l'attribut structurant ou tableau contenant cet
attribut.

Le type *tab* ne peut pas avoir d'attribut conteneur.

Le type *frame* peut être contenu dans un *tab*, ou ne pas avoir d'attribut
conteneur.

Le type *array* doit être contenu dans un *frame*.

Tous les autres type d'attributs doivent être contenus dans 
un *array* ou un  *frame*.

#### Caractéristique `[label]` {#core-ref:29e74e07-09ff-49f3-8168-1cc1777d37ad}

**facultatif**  
Libellé de l'attribut.

Ce libellé sera automatiquement traduit s'il est présent dans le catalogue
de traduction (la clé correspondante est de la forme `[FAMNAME]#[attrid]`,
avec [FAMNAME] le nom logique de la famille et
[attrid] l'identifiant de l'attribut, en minuscule).

#### Caractéristique `[in_title]` {#core-ref:b0e414c0-b795-4bbe-b70e-a308b7f1b4ab}

**Obligatoire** (Non applicable pour les types *array*, *frame* ou *tab*)  
Indique que l'attribut sera utilisé dans la composition du titre du document.

Le titre du document est alors composé en concaténant toutes les valeurs
**brutes** d'attributs définis, par ordre croissant de leur *ordre* et en les
séparant par des espaces. Les options de formatage des attributs ne sont pas
pris en compte pour le titre.

Cette caractéristique est ignorée sur les attributs de type 
*array*, *frame* ou *tab*.

Si aucun attribut n'est marqué comme composant le titre, ou si tous ces
attributs sont vides, le titre sera *Document sans titre* suivi de 
l'identifiant interne du document.

Les valeurs possibles sont :

*   `Y` pour *yes*
*   `N` pour *no*

La composition du titre peut aussi être définie par programmation en
surchargeant la méthode [Doc::getCustomTitle()][getcustomtitle].

Le titre d'un document ne peut excéder **255** caractères. Il est
automatiquement tronqué si cette limite est atteinte.

La visibilité des attributs n'est pas prise en compte : un attribut en visibilité
cachée mais ayant la colonne titre à Y est visible dans le titre.

Les valeurs des attributs multiples seront concaténées et séparées par un espace.

#### Caractéristique `[in_abstract]` {#core-ref:39825a45-a204-440b-ab0c-608e765eb88c}

**Obligatoire** (Non applicable pour les types  *menu*, *array*, *frame* ou *tab*)  
Indique que l'attribut sera utilisé dans le résumé du document.

Le résumé du document est utilisé pour construire la fiche résumé dans
l'application ONEFAM.

Les valeurs possibles sont :

*   `Y` pour *yes*
*   `N` pour *no*

#### Caractéristique `[type]` {#core-ref:a371c892-2749-49a2-831f-0d1f470b9f61}

**Obligatoire** 
Indique le type de l'attribut.
Les types d'attributs supportés sont définis au chapitre [Type de l'attribut][type_attribut].

Cette colonne permet également de définir le formatage de l'attribut :

*   attributs de type *text*
    
    Le formatage est effectué avec la [syntaxe sprintf][PHP_sprintf].
    
    Par exemple :
    *   `text("%s environ")` ajoute *environ* après la valeur ;
    *   `text("<b>%s</b>")` affiche la valeur en gras.

*   attributs de type *int* ou *double*
    
    Le formatage est effectué avec la [syntaxe sprintf][PHP_sprintf].
    
    Par exemple :
    *   `double("%.02f")` affiche le nombre avec 2 décimales ;
    *   `integer("%d m³")` affiche la valeur suivie de *m³* ;
    *   `double("%.02f %%")` affiche *%* après le nombre.

*   attributs de type *date*, *time* et *timestamp*
    
    Le formatage est effectué avec la syntaxe [strftime][PHP_strftime].
    
    Par exemple, `time("%H:%M:%S")`, ou `timestamp("%A %d %B %Y %X")`.


#### Caractéristique `[ordre]` {#core-ref:0833f7ea-e25d-49d9-8648-4a9c13ab9f2d}

**Obligatoire**   
Les attributs ont un ordre dans la structure. Cet ordre est utilisé pour
représenter le document mais il est aussi utilisé pour ordonner le calcul des
attributs ([phpfunc][phpfunc]) et pour la composition du [titre][titleis].



##### Ordre relatif  {#core-ref:6b4b44c9-8fd6-4154-9d11-fff2a7b02523}

<span class="flag from release inline">3.2.23</span> Pour que l'ordre suive 
l'ordre de la déclaration des attributs dans le fichier
de déclaration il faut indiquer le mot-clef `::auto`.

Soit la famille AA :

| BEGIN |            | The a  |  AA |            |
| ----- | ---------- | ------ | --- | ---------- |
| //    | attributes | parent | ... | **order**  |
| ATTR  | A1         |        |     | **::auto** |
| ATTR  | A2         | A1     | ... | **::auto** |
| ATTR  | A3         | A1     | ... | **::auto** |
| ATTR  | A4         | A1     | ... | **::auto** |
| ATTR  | A5         | A4     | ... | **::auto** |
| ATTR  | A6         |        | ... | **::auto** |
| ATTR  | A7         | A6     | ... | **::auto** |

Dans ce cas, la structure résultante de AA est :

*  A1
    *  A2
    *  A3
    *  A4
        *  A5
*  A6
    *  A7

En cas de surcharge de famille ou d'héritage de famille, l'ordre permet
d'indiquer où le nouvel attribut sera inséré. Le mot-clef `::auto`
indique que l'attribut sera inséré à la fin de la structure de l'attribut
englobant (cadre, onglet, tableau), s'il n'y a pas d'attribut englobant, il sera
inséré à la fin du document.

Sot la famille BA héritant de la famille AA :

| BEGIN |     AA     | The b  |  BA |            |
| ----- | ---------- | ------ | --- | ---------- |
| //    | attributes | parent | ... | **order**  |
| ATTR  | B1         |        |     | **::auto** |
| ATTR  | B2         | B1     | ... | **::auto** |
| ATTR  | B3         | B1     | ... | **::auto** |
| ATTR  | B4         |        | ... | **::auto** |
| ATTR  | B5         | B5     | ... | **::auto** |
| ATTR  | B6         | A6     | ... | **::auto** |
| ATTR  | B7         | A4     | ... | **::auto** |
| ATTR  | B8         | A4     | ... | **::auto** |

La structure résultante de BA est :

*  A1
    *  A2
    *  A3
    *  A4
        *  A5
        *  B7
        *  B8
*  A6
    *  A7
    *  B6
*  B1
    *  B2
    *  B3
*  B4
    *  B5

Le mot-clef `::first`, permet d'insérer un attribut en premier dans la structure
englobante.

Soit la famille CA héritant de la famille AA :

| BEGIN |     AA     | The c  |  CA |             |
| ----- | ---------- | ------ | --- | ----------- |
| //    | attributes | parent | ... | **order**   |
| ATTR  | C1         |        |     | **::first** |
| ATTR  | C2         | C1     | ... | ::auto      |
| ATTR  | C3         | C1     | ... | ::auto      |
| ATTR  | C4         |        | ... | **::first** |
| ATTR  | C5         | C5     | ... | ::auto      |
| ATTR  | C6         | A4     | ... | **::first** |
| ATTR  | C7         | A4     | ... | **::first** |

La structure résultante de CA est :

*  C4
    *  C5
*  C1
    *  C2
    *  C3
*  A1
    *  A2
    *  A3
    *  A4
        *  C7
        *  C6
        *  A5
*  A6
    *  A7

Dans ce cas, on remarque que B1 est derrière B4 alors qu'il a été déclaré avant
B4 dans le fichier. Ceci est du à l'ordre `::first` qui est interprété dans
l'ordre de la déclaration. Au moment de l'interprétation B1 était déjà inséré.
Le même principe est visible pour les attributs C6 et C7.

L'ordre peut contenir une référence à un attribut. Cette référence indique que
l'attribut sera placé après cette référence. Cette référence ne peut être qu'un
attribut de même profondeur.

Soit la famille DA héritant de la famille AA :

| BEGIN |     AA     | The d  |  DA |           |
| ----- | ---------- | ------ | --- | --------- |
| //    | attributes | parent | ... | **order** |
| ATTR  | D1         |        |     | **A1**    |
| ATTR  | D2         | D1     | ... | ::auto    |
| ATTR  | D3         | D1     | ... | ::auto    |
| ATTR  | D4         |        | ... | **D1**    |
| ATTR  | D5         | D5     | ... | ::auto    |

La structure résultante de DA est :

*  A1
    *  A2
    *  A3
    *  A4
        *  A5
*  D1
    *  D2
    *  D3
*  D4
    *  D5
*  A6
    *  A7

L'attribut D1 est inséré après A1. L'attribut D4 est inséré après D1.

L'ordre est appliqué suivant la hierarchie des héritages. C'est à dire que les
ordres d'une famille fille sont calculés à partir des ordres calculés de la
famille mère.

Soit la famille EA héritant de la famille AA :

|  BEGIN  |     AA     | The d  |  EA |           |
| ------- | ---------- | ------ | --- | --------- |
| //      | attributes | parent | ... | **order** |
| ATTR    | E1         |        |     | ::auto    |
| ATTR    | E2         | D1     | ... | ::auto    |
| MODATTR | A1         |        | ... | **A6**    |

Soit la famille FE héritant de la famille EA :

|  BEGIN  |     EA     | The d  |  FE |           |
| ------- | ---------- | ------ | --- | --------- |
| //      | attributes | parent | ... | **order** |
| ATTR    | F1         |        |     | **E1**    |
| ATTR    | F2         | F1     | ... | ::auto    |
| MODATTR | E1         |        | ... | **A6**    |


La structure résultante de EA est :

*  A6
    *  A7
*  A1
    *  A2
    *  A3
    *  A4
        *  A5
*  E1
    *  E2

La structure résultante de FE est :

*  A6
    *  A7
*  E1
    *  E2
*  F1
    *  F2
*  A1
    *  A2
    *  A3
    *  A4
        *  A5


##### Ordre absolu {#core-ref:eb3da9f5-c277-4312-bac1-f14276e8e9bb}

L'ordre est un nombre entier.(Non applicable pour les types *frame* ou *tab*)

<span class="flag inline nota-bene"/> : Les tableaux (type `array`) doivent
toujours avoir un ordre inférieur aux attributs qui le composent.

Dans le cas d'un nombre, cette caractéristique est ignorée sur les 
attributs de type *frame* ou *tab*.

<span class="flag inline nota-bene"/> Il est déconseillé de mélanger des ordres
absolus et relatifs sur une même famille. Dans ce cas, l'ordre résultant ne sera 
pas pertinent.

La structure suit l'ordre numérique donné.

Soit la famille AN :

| BEGIN |            | The a  |  AN |       |           |
| ----- | ---------- | ------ | --- | ----- | --------- |
| //    | attributes | parent | ... | type  | **order** |
| ATTR  | A1         |        |     | frame |           |
| ATTR  | A2         | A1     | ... | text  | 10        |
| ATTR  | A3         | A1     | ... | text  | 20        |
| ATTR  | A4         | A1     | ... | array | 30        |
| ATTR  | A5         | A4     | ... | text  | 40        |
| ATTR  | A6         |        | ... | frame |           |
| ATTR  | A7         | A6     | ... | text  | 50        |


Dans ce cas la structure résultante de AN est :

*  A1
    *  A2
    *  A3
    *  A4
        *  A5
*  A6
    *  A7

Dans ce cas, les ordres des attributs englobant (tab et frame) sont déduits de
l'ordre absolu de l'attribut.

Soit la famille BN :

| BEGIN |            | The b  |  BN |       |           |
| ----- | ---------- | ------ | --- | ----- | --------- |
| //    | attributes | parent | ... | type  | **order** |
| ATTR  | B1         |        |     | frame |           |
| ATTR  | B2         | B1     | ... | text  | 100       |
| ATTR  | B3         | B1     | ... | text  | 110       |
| ATTR  | B4         | B1     | ... | array | 120       |
| ATTR  | B5         | B4     | ... | text  | 130       |
| ATTR  | B6         |        | ... | frame |           |
| ATTR  | B7         | B6     | ... | text  | 10        |

La structure résultante de BN est :

*  B6
    *  B7
*  B1
    *  B2
    *  B3
    *  B4
        *  B5

En cas de modification d'un ordre d'un attribut structurant, il est nécessaire de
modifier tous les ordres des attributs le composant. 

Soit la famille AN :

|  BEGIN  |     CN     | The a  |  AN |       |           |
| ------- | ---------- | ------ | --- | ----- | --------- |
| //      | attributes | parent | ... | type  | **order** |
| MODATTR | A2         | A1     | ... | text  | 100       |
| MODATTR | A3         | A1     | ... | text  | 110       |
| MODATTR | A4         | A1     | ... | array | 120       |
| MODATTR | A5         | A4     | ... | text  | 130       |
| ATTR    | C1         |        | ... | frame |           |
| ATTR    | C2         | A6     | ... | text  | 10        |

La structure résultante de CN est :

*  C1
    *  C2
*  A6
    *  A7
*  A1
    *  A2
    *  A3
    *  A4
        *  A5

<span class="flag from release inline">3.2.23</span> Il n'est plus possible d'avoir
des cadres doublé (structure scindée).

Soit la famille DN dont l'attribut A3 à un ordre supérieur au frère de son père.

| BEGIN |            | The d  |  DN |       |           |
| ----- | ---------- | ------ | --- | ----- | --------- |
| //    | attributes | parent | ... | type  | **order** |
| ATTR  | A1         |        |     | frame |           |
| ATTR  | A2         | A1     | ... | text  | 10        |
| ATTR  | A3         | A1     | ... | text  | **100**   |
| ATTR  | A4         | A1     | ... | array | 30        |
| ATTR  | A5         | A4     | ... | text  | 40        |
| ATTR  | A6         |        | ... | frame |           |
| ATTR  | A7         | A6     | ... | text  | 50        |


<span class="flag until release inline">3.2.22</span> Avant : Le cadre était doublé
pour respecter l'ordre absolu.

*  A1
    *  A2
    *  A4
        *  A5
*  A6
    *  A7
*  A1
    *  A3

<span class="flag from release inline">3.2.23</span> Maintenant : L'ordre 
calculé du cadre est fonction du maximum des ordres des attributs fils.

*  A6
    *  A7
*  A1
    *  A2
    *  A3
    *  A4
        *  A5


#### Caractéristique `[visibility]` {#core-ref:6e98d2fb-4327-4679-9cfa-5724b9101992}

**Obligatoire**  
Définit la [visibilité][visibility] par défaut de l'attribut dans les interfaces
web de consultation et de modification du document.

Les valeurs possibles sont :

*   `H` (*Hidden*) : attribut caché.
    Généralement utilisé pour des attributs servant soit au calcul, soit à
    la génération des liens.
*   `I` (*Invisible*) : attribut invisible :
    l’attribut n’est présent ni en consultation ni en modification dans le
    document.  
    Un attribut de visibilité I n'est pas modifiable et pour en modifier la
    valeur, il est nécessaire de modifier sa visibilité.
*   `O` :
    attribut modifiable en modification mais non visible en lecture.
*   `R` (*Read-only*) : attribut visible en lecture seulement.
    Généralement utilisé pour les attributs calculés.
*   `S` (*Statique*) : attribut visible en lecture et en modification, mais
    non modifiable en modification.
*   `W` (*Writable*) : attribut visible en lecture et modifiable en
    modification.

Le type `array` dispose en plus de la visibilité

*   `U` : Interdit l'ajout et la suppression de lignes dans le tableau.

Cette caractéristique peut être modifiée par les [masques][masque] 
des [contrôle de vues][control_de_vue].

#### Caractéristique `[required]` {#core-ref:8c74cf5b-3e03-480f-ba05-1a86ea6ec634}

**facultatif**  (Non applicable pour les types "frame", "tab", "array", "menu")

Indique si l'attribut est obligatoire pour la sauvegarde du document 
depuis l'interface web de modification du document. 
Cette caractéristique n'est pas prise en compte lors des sauvegardes faite
par le code (méthode `Doc::store()`) ni lors de l'importation de document.
Par contre, cette caractéristique est prise en compte lors d'un passage de
transition (document lié par un cycle de vie -_workflow_-).  
Cette caractéristique peut être modifiée par les [masques][masque] 
des [contrôle de vues][control_de_vue].

<span class="flag from release">3.2.19</span> Pour les attributs inclus dans un
tableau, cette caractéristique indique que la valeur doit être renseignée pour
chacune des rangées du tableau. Si aucune rangée, n'est indiquée dans le tableau,
le document pourra être sauvé même si aucune valeur n'est renseignée. Le nombre
de rangée minimum peut être indiqué à l'aide d'une [contrainte][constraint].

Les valeurs possibles sont :
    
*   `Y` pour *yes*
*   **`N` (valeur par défaut)** pour *no*

#### Caractéristique `[link]` {#core-ref:aaaa5d78-0982-4c3e-a8ed-a125c49572a8}

**facultatif** (Non applicable pour les types "frame", "tab", "array")  
Ajoute un hyperlien sur l'attribut sur les interfaces web de consultation des documents.

Par défaut, le texte de l'hyperlien est la valeur de l'attribut. L'option
`ltarget` permet de changer ce texte.

L'hyperlien peut être :

*   une URL statique ("http://www.dynacase.org"),
*   une URL paramétrée par
    *   des attributs du document,
    *   des propriétés du document,
    *   des paramètres applicatifs,
   *   des méthodes du document.

*   Utilisation d'attribut du document
    
    Les références aux attributs du document sont écrites entre les
    caractères `%`, en indiquant l'identifiant de l'attribut, en majuscules
    ou en minuscules.
    
    Le caractère `%` est obtenu en le doublant : `TEST%%25` sera converti en
    *TEST%25*.
    
    Par exemple, soit l'attribut US_MAIL définissant le mail d'une personne.
    Pour déclencher l'édition d'un mail vers une personne,
    il suffit de mettre l'hyperlien suivant : `mailto:%US_MAIL%`.
    
    Autre exemple, soit l'attribut SI_TOWN indiquant la ville de la famille
    société.  Pour avoir la météo de la ville 
    il suffit de mettre l'hyperlien suivant :
    `http://www.location.org/&strLocation=%SI_TOWN%&strCountry=EUR`.
    
    Si la valeur est vide pour un des attributs de l'URL,
    l'hyperlien ne sera pas affiché (on ne pourra pas cliquer sur
    l'attribut).
    
    Il est possible de désigner des attributs optionnels en utilisant la
    notation suivante : `%?[ATTRID]%`.
    Le point d'interrogation placé après le pourcentage indique que la
    valeur de l'attribut peut être vide.
    
    Les valeurs insérées dans le lien sont encodés selon la
    [RFC 3986][RFC_3986] pour être utilisable comme valeur de paramètre web.

*   Utilisation de propriétés du document
    
    Les références aux propriétés du document sont écrites entre les
    caractères `%`, en indiquant l'identifiant de l'attribut, en majuscules
    ou en minuscules.
    
    Le caractère `%` est obtenu en le doublant : `TEST%%25` sera converti en
    *TEST%25*.
    
    Les mots-clefs spéciaux suivants peuvent être utilisés pour la
    composition de l'URL :
    
    *   `%S%` : est remplacé par l'URL relative vers dynacase,
    *   `%U%` : est remplacé par l'[URL absolue][urlindex] <span class="flag from release inline">3.2.21</span>,
    *   `%I%` : est remplacé par l'identifiant (équivalent à %ID%),
    *   `%T%` : est remplacé par le titre (équivalent à %TITLE%).
    
    Les valeurs insérées dans le lien sont encodés selon la
    [RFC 3986][RFC_3986] pour être utilisable comme valeur de paramètre web.

*   Utilisation de paramètres applicatifs
    
    Les références aux paramètres applicatifs sont écrites entre accolades,
    en indiquant l'identifiant de l'attribut, en majuscules ou en
    minuscules.
    
    Par exemple : `%S%app=TEST&action=TESTONE&arg={CORE_CLIENT}` :
    ici {CORE_CLIENT} sera remplacé par la valeur du paramètre CORE_CLIENT.
    
    Seuls les paramètres de *CORE* et les *paramètres globaux* sont
    accessibles pour la composition de l'URL.

*   Utilisation de méthodes du document
    
    Les références aux méthodes du document sont écrites entre caractères `%`,
    en notant la méthode comme pour les attributs calculés.
    
    Les parties variables peuvent aussi faire référence à une méthode du
    document.
    Cette méthode doit retourner une chaîne de caractère encodée selon la
    [RFC 3986][RFC_3986] qui sera insérée dans la variable.
    
    Par exemple : `%::myTitleLink()%&x=3`
    
        [php]
        public function myTitleLink(){
            return sprintf('http://www.example.net/?b=%s',
                rawurlencode($this->getTitle())
            );
        }

#### Caractéristique `[phpfile]` {#core-ref:7362e2ff-cfb5-45f0-a81d-e02eab6d0fb6}

**Facultatif** (Non applicable pour les types "frame", "tab", "array", "menu")

Nom du fichier php pour l'[aide à la saisie][aide_saisie].
Ce fichier doit être présent dans le répertoire `EXTERNALS`.

#### Caractéristique `[phpfunc]` {#core-ref:1128e658-48f5-440f-9fd1-2d714e99eecd}

**Facultatif** (Non applicable pour les types "frame", "tab", "array")

Cette caractéristique est utilisée pour trois usages différents :

##### Définition d'une aide à la saisie {#core-ref:a23b0985-70a6-48f0-9724-2368905eb58e}

Nom et attributs de la fonction pour l'[aide à la saisie][aide_saisie].  
Dans ce cas `[phpfile][phpfile]` doit être défini.

##### Définition d'un attribut calculé {#core-ref:8688316c-6ebf-4c5d-91be-99621a1414c2}

Nom et attributs de la méthode de calcul s'il s'agit d'un
[attribut calculé][attribut_calcule].  
Dans ce cas `[phpfile][phpfile]` doit être vide.

##### Définition des valeurs d'un énuméré {#core-ref:cd115566-a75d-46b2-908f-00a95ba79f9f}

Ne concerne que le cas des attributs de type `enum`.

[Couples *clé*/*label*][def_enum] correspondant à l'énuméré.

##### Définition de la visibilité d'un menu {#core-ref:0865183e-8356-47bb-a4ee-160e622445ab}

Ne concerne que le cas des attributs de type `menu`.

Méthode à utiliser pour la [visibilité][menuvis] du menu.

#### Caractéristique `[elink]` {#core-ref:edf84026-7980-442f-bc86-88739e49e3b5}

**facultatif** (Non applicable pour les types "frame", "tab", "array", "menu")  
Extra lien supplémentaire.

Cet extra lien sera présenté dans les interfaces web de modification de
document, sous la forme d'un bouton supplémentaire.

La syntaxe de l'extra lien est la même que celle de la colonne *link*.

#### Caractéristique `[constraint]` {#core-ref:f0177c62-1774-4724-a337-f090406e2d08}

**facultatif** (Non applicable pour les types "frame", "tab")  
Nom et attributs de la méthode de [contrainte][contrainte].

#### Caractéristique `[options]` {#core-ref:16f74982-e9f6-4d47-a582-79348fe8ffb7}

**facultatif**  
Liste des options à appliquer à l'attribut. Les options dépendent du [type d'attribut][type_attribut].

Les options se présentent sous la forme de couples `clé=valeur` séparés par des `|`.

Par exemple: `esize=3|elabel=saisissez votre prénom`

### Définition de paramètres de famille {#core-ref:c28824e2-3486-11e2-be3b-337d2321d8ee}

Un [paramètre de famille][family_param] est défini par la syntaxe suivante :

    PARAM;[id_attribut];[id_conteneur];[label];[in_title];[in_abstract];[type];[ordre];[visibility];[required];[link];[phpfile];[phpfunc];[elink];[constraint];[options]

avec les mêmes correspondances que pour [les attributs][attributs],
aux exceptions suivantes :

*   Pour les paramètres calculés, le calcul est fait lors de l'accès au
    paramètre.
*   Les méthodes de calcul et de contrainte doivent être statiques (elles sont
    spécifiées par la syntaxe `class::method`).
*   Le caractère obligatoire n'est applicable que dans le cadre de l'utilisation
    du [paramètre][wask] dans un cycle de vie. Il n'est pas pris en compte dans 
    les interfaces d'administration des paramètres.

### Définition de valeurs par défaut {#core-ref:94fa51e2-3488-11e2-9e34-1f7c912168cf}

Les valeurs par défaut s'appliquent aux attributs comme aux paramètres.

Appliquées aux attributs, elles déterminent la valeur de l'attribut lors de la
création d'un nouveau document, alors qu'appliquées aux paramètres,
elles déterminent la valeur du paramètre lorsque l'utilisateur n'a saisi aucune
valeur.

La valeur par défaut d'un attribut peut être modifiée depuis le document de la
famille via l'interface "Valeurs par défaut et paramètres" puis "Modifier les
valeurs par défaut".

Une valeur par défaut est définie par la syntaxe suivante :

    DEFAULT;[attrid|paramid];[value];force=yes

avec les correspondances suivantes :

DEFAULT
:   **Obligatoire**  
    Signale que la ligne est une définition de valeur par défaut.

[attrid|paramid]
:   **Obligatoire**  
    Identifiant de l'attribut ou du paramètre auquel s'applique la valeur par
    défaut.

[value]
:   La valeur par défaut, statique, ou la méthode de calcul.
    
    Dans le cas d'une méthode de calcul, sa syntaxe est identique à celle d'un
    [attribut calculé][attribut_calcule].
    
    Par exemple, `DEFAULT;SGATE_RED;::getTitle(SGATE_IDRED)`
    pré-remplit l'attribut  avec le titre de l'attribut *SGATE_IDRED*,
    alors que `DEFAULT;SGATE_ACTION;GATE_WEATHER`
    pré-remplit l'attribut SGATE_RED avec la chaîne *GATE_WEATHER*.
    
    Cas particulier des *array*:
    
    *   Pour définir quelles lignes seront présentes par défaut dans le tableau,
        on spécifie la valeur par défaut de l'attribut *array*.
        
        Cette valeur par défaut est une structure JSON sous la forme d'un
        tableau d'objets.
        
        Par exemple,
        `DEFAULT;ATTR_ARRAY;[{"c1":"1.1", "c2":"1.2"},{"c1":"2.1", "c2":"2.2"}]`
        indique que le tableau *ATTR_ARRAY* sera pré-rempli avec 2 lignes.
        
        *   La première ligne aura comme valeurs
            *   *1.1* dans la colonne correspondant à l'attribut *c1*
            *   *1.2* dans la colonne correspondant à l'attribut *c2*
        *   La seconde ligne aura comme valeurs
            *   *2.1* dans la colonne correspondant à l'attribut *c1*
            *   *2.2* dans la colonne correspondant à l'attribut *c2*
        
        Si l'on souhaite utiliser une méthode de calcul pour cette valeur par
        défaut, la méthode doit retourner le tableau à 2 dimensions qui, une
        fois json_encodé, donne la structure précédente.
        
        Par exemple, la méthode correspondant à la valeur par défaut précédente
        sera `DEFAULT;ATTR_ARRAY;::defaultArrayValue()`.
        
            [php]
            public function defaultArrayValue(){
                return array(
                    array(
                        "c1" => "1.1",
                        "c2" => "1.2"
                    ),
                    array(
                        "c1" => "2.1",
                        "c2" => "2.2"
                    )
                );
            }
    
    *   Pour définir les valeurs par défaut de chaque cellule lors de
        l'ajout d'une nouvelle ligne, on spécifie la valeur par défaut de
        l'attribut correspondant à cette colonne.  
        De fait, la valeur par défaut d'un attribut contenu dans un *array* doit
        être simple.

[force=yes]
:   **facultatif** Indique, lors de la surcharge de valeur par défaut, que la
    nouvelle valeur par défaut écrase l'ancienne.
    
    Dans le cas contraire, la nouvelle valeur par défaut n'est pas prise en
    compte lors des mises à jours.
    
    Les valeurs par défaut peuvent être réinitialisées avec la clef
     [`RESET default`][reset].

### Définition de valeur initiale de paramètre {#core-ref:da804e2e-3573-11e2-8974-4ba96567fbf9}

Les paramètres de familles peuvent avoir des valeurs par défaut,
utilisées lorsque ces paramètres n'ont pas de valeur explicite ;
mais il est parfois nécessaire de définir la valeur initiale du paramètre en
plus de sa valeur par défaut.

La valeur d'un paramètre peut être modifiée depuis le document de la
famille via l'interface "Valeurs par défaut et paramètres" puis "Modifier les
paramètres".

Une valeur initiale de paramètre est définie par la syntaxe suivante :

    INITIAL;[paramid];[value];force=yes

avec les correspondances suivantes :

INITIAL
:   **Obligatoire**  
    Signale que la ligne est une définition de valeur initiale de paramètre.

[paramid]
:   **Obligatoire**  
    identifiant du paramètre auquel s'applique la valeur initiale.

[value]
:   la valeur initiale, statique, ou la méthode de calcul.
    
    Dans le cas d'une méthode de calcul, sa syntaxe est identique à celle d'un
    [attribut calculé][attribut_calcule].

[force=yes]
:   Indique, lors de la surcharge de valeur initiale, que la nouvelle valeur
    initiale écrase l'ancienne.
    
    Dans le cas contraire, la nouvelle valeur initiale n'est pas prise en compte.
    Les valeurs initiales peuvent être réinitialisées avec la clef
     [`RESET parameters`][reset].

### Modification d'attribut père {#core-ref:13dcd96d-561f-463c-a88f-4b9db58e4fbd}

Lorsqu'une famille étend une autre famille, il peut être nécessaire de changer
la définition d'un attribut. Cela se fait au moyen d'une ligne de la forme :

    MODATTR;[id_attribut];[id_conteneur];[label];[in_title];[in_abstract];[type];[ordre];[visibility];[required];[link];[phpfile];[phpfunc];[elink];[constraint];[options]

avec les même correspondances que pour la
[définition d'un attribut][definition_attribut], à quelques différences près :

#### MODATTR {#core-ref:231ae6fe-7179-494e-aedf-949f8ef247d3}

**Obligatoire**  
Signale que la ligne est une modification d'attribut.

#### Caractéristique `[id_attribut]` {#core-ref:d09609df-0609-4cb5-b3d6-097adc7c38d9}

**Obligatoire**  
Identifiant système de l'attribut à surcharger.
Il sera automatiquement converti en minuscules.

L'attribut référencé doit obligatoirement exister dans la famille étendue.

#### Caractéristique `[id_conteneur]` {#core-ref:01e6587d-8cfa-489d-92e4-869ac7d1394c}

Identifiant système de l'attribut structurant ou tableau contenant cet
attribut.

Si la valeur est vide, l'attribut conservera sa valeur héritée.

#### Caractéristique `[label]` {#core-ref:fdc1daef-d630-4b2b-a34a-bd0b0bb6b156}

Libellé de l'attribut.

Si la valeur est vide, l'attribut conservera sa valeur héritée.

#### Caractéristique `[in_title]` {#core-ref:f34b9486-6a00-44d8-b0c0-e83e3336ba34}

Indique que l'attribut sera utilisé dans la composition du titre du document.

Si la valeur est vide, l'attribut conservera sa valeur héritée.

#### Caractéristique `[in_abstract]` {#core-ref:7ed2bf36-1fc0-4ab3-ac99-45902e984db1}

Indique que l'attribut sera utilisé dans le résumé du document.

Si la valeur est vide, l'attribut conservera sa valeur héritée.

#### Caractéristique `[type]` {#core-ref:62b4a0b2-85b3-49f3-945d-1c2fe0383fcb}

[type de l'attribut][type_attribut]

Si la valeur est vide, l'attribut conservera sa valeur héritée.

Le nouveau type d'attribut doit être compatible avec l'ancien
(son stockage en base de données doit être de même type).

#### Caractéristique `[ordre]` {#core-ref:f6ada4de-6fcd-4c11-b62a-5dca83b4c98e}

Définit l'ordre de présentation des attributs dans le document.

Si la valeur est vide, l'attribut conservera sa valeur héritée.

#### Caractéristique `[visibility]` {#core-ref:4050923d-2fb2-4804-a304-d8fc9a60afc1}

Définit la [visibilité][visibility] par défaut de l'attribut.

Si la valeur est vide, l'attribut conservera sa valeur héritée.


#### Caractéristique `[required]` {#core-ref:baea51d2-7730-4f4b-b351-16e1312ac7cd}

Indique si l'attribut est obligatoire pour la sauvegarde du document.

Si la valeur est vide, l'attribut conservera sa valeur héritée.

#### Caractéristique `[link]` {#core-ref:79eae539-d5fe-438a-a88a-838ec32e476e}

Ajoute un hyperlien sur l'attribut (en consultation uniquement).

Si la valeur est vide, l'attribut conservera sa valeur héritée.

Pour supprimer la valeur, il faut mettre `-`.

#### Caractéristique `[phpfile]` {#core-ref:880a365f-4407-44cf-bf43-9fbfc1715d70}

Nom du fichier php pour l'[aide à la saisie][aide_saisie].

Si la valeur est vide, l'attribut conserve sa valeur héritée.

Pour supprimer la valeur, il faut mettre `-`.

#### Caractéristique `[phpfunc]` {#core-ref:a847fb60-f96a-4deb-8e35-ac2c7f028455}

Nom et attributs de la fonction pour l'aide à la saisie,
ou nom et attributs de la méthode de calcul s'il s'agit d'un attribut
calculé, ou définition des clés-valeurs dans le cas d'un attribut de type
énuméré.

Si la valeur est vide, l'attribut conserve sa valeur héritée.

Pour supprimer la valeur, il faut mettre `-`.

#### Caractéristique `[elink]` {#core-ref:4e16191a-7686-436b-a31a-65127b48413e}

Extra lien supplémentaire.

Si la valeur est vide, l'attribut conserve sa valeur héritée.

Pour supprimer la valeur, il faut mettre `-`..

#### Caractéristique `[constraint]` {#core-ref:1aa481ee-c90b-4e97-8979-750d30c92408}

Nom et attributs de la méthode de [contrainte][contrainte].

Si la valeur est vide, l'attribut conserve sa valeur héritée.

Pour supprimer la valeur, il faut mettre `-`.

#### Caractéristique `[options]` {#core-ref:c82cd1bc-a5eb-4f19-b0a2-81542ec4c922}

Liste des options à appliquer à l'attribut.

Si la valeur est vide, l'attribut conserve sa valeur héritée.

Pour supprimer la valeur, il faut mettre `-`.

Afin de modifier la valeur d'une seule option, il est nécessaire de toutes
les reporter, car la valeur `-` va effacer entièrement le contenu de la
colonne *options*.

### Instructions de réinitialisation {#core-ref:5c661733-772d-42b8-8b3e-b70453ddfd33}

Lors de la [surcharge de famille](#core-ref:686b17c3-220e-4450-a0d0-feb5db002cbc),
il est possible de réinitialiser certaines parties de la famille.

La syntaxe est la suivante :

    RESET;[keyword]

avec les correspondantes suivantes :

RESET
:   **Obligatoire**  
    Signale que la ligne est une réinitialisation.

[keyword]
:   **Obligatoire**  
    Indique quels éléments doivent être réinitialisés.
    
    Les valeurs possibles sont :
    
    `attributes`
    :   Réinitialise les attributs.
        
        La définition de tous les attributs sera effacée
        (les valeurs ne sont pas supprimées des documents existants).
        
        **Attention** : la définition des paramètres de la famille est également
        effacée (bien que les valeurs soient également conservées).
    
    `default`
    :   Réinitialise les valeurs par défaut.
        
        Toutes les valeurs par défaut sont effacées.
    
    `properties`
    :   Réinitialise les valeurs des paramètres des propriétés.
        
        Tous les paramètres de propriétés seront remis à leur valeur par défaut.
    
    `parameters`
    :   Réinitialise les valeurs des paramètres de famille
        
        Toutes les valeurs des paramètres de famille sont effacées. Ces 
        paramètres utiliseront les valeurs par défaut si elles sont définies.
    
    `structure`
    :   Réinitialise la liste des attributs aux seuls attributs
        définis entre le `BEGIN` et le `END` de la famille.
        
        Les définitions des attributs qui ne sont plus définis sont supprimés.
        Cela veut dire que non seulement la définition de l'attribut est
        supprimé mais aussi les valeurs de ces attributs sur tous les documents
        de la famille et des familles dérivées. Cette action est
        **irréversible**.
    
    `enums`
    :   Réinitialise les définitions des énumérés de la famille. 
    
    Les nouvelles définitions remplaceront les anciennes définitions. Les
    énumérés définis par les utilisateurs ou les administrateurs seront
    perdus.


Ces instructions de réinitialisation doivent être placées avant la nouvelle
définition des éléments réinitialisés.

Chaque instruction doit être sur sa propre ligne.

## Surcharge de définition de famille {#core-ref:686b17c3-220e-4450-a0d0-feb5db002cbc}

La définition d'une famille peut être surchargée, permettant ainsi une
importation plus modulaire.
Il est bien question ici de la notion de *surcharge* qui change la définition
d'une famille, et à ne pas confondre avec le mécanisme d'héritage, qui crée une
nouvelle famille.

Pour surcharger une définition de famille, il suffit d'utiliser le même nom
logique (sur la ligne *BEGIN*).

Lors de la surcharge, le comportement est le suivant :

*   pour les [paramètres de propriété](#core-ref:40d229c4-33c4-11e2-9147-a3eaf356c37c)
    
    La nouvelle valeur écrase la valeur précédemment définie.

*   pour les [propriétés de famille](#core-ref:6f013eb8-33c7-11e2-be43-373b9514dea3)
    
    La nouvelle valeur écrase la valeur précédemment définie.

*   pour les [attributs](#core-ref:bc3fad86-33cc-11e2-9a69-1bbd9c32b0f2)
    
    La nouvelle valeur écrase la valeur précédemment définie.

*   pour les [paramètres de famille](#core-ref:c28824e2-3486-11e2-be3b-337d2321d8ee)
    
    La nouvelle valeur écrase la valeur précédemment définie.

*   pour les [valeurs par défaut](#core-ref:94fa51e2-3488-11e2-9e34-1f7c912168cf)
    
    La nouvelle valeur écrase la valeur précédemment définie si *force=yes*
     est utilisé.

*   pour les [valeurs initiale de paramètre](#core-ref:da804e2e-3573-11e2-8974-4ba96567fbf9)
    
    La nouvelle valeur écrase la valeur précédemment définie si *force=yes* 
    est utilisé.

Ce comportement peut être altéré par l'utilisation des
[Instructions de réinitialisation][reset].

## Prise en compte d'une définition de famille dans Dynacase {#core-ref:a2a64666-660f-43ca-8edf-943d0037f0df}

Une fois votre définition de famille créée, vous pouvez l'importer dans Dynacase.

Cette importation permettra à Dynacase de créer les tables nécessaires aux
documents de cette famille, et de rendre la dite famille disponible dans les
différentes IHM de Dynacase.

L'importation se fait en ligne de commande, avec la commande suivante :

    ./wsh.php --api=importDocuments --file=[chemin vers le fichier de définition]

Pour plus de détails sur l'API `importDocuments`, se référer à sa
[documentation][importDocuments]

## Erreur d'importation d'attributs {#core-ref:30ec0560-cd68-443a-aa00-587d58a1eb93}

La liste des codes d'erreur possibles lors de l'importation d'attributs est
consultable dans la documentation de l'API PHP : [ErrorCodeATTR Class
Reference][ErrorCodeATTR].

<!-- links -->
[PHP_sprintf]: http://php.net/manual/fr/function.sprintf.php "Documentation de la fonction sprintf sur php.net"
[PHP_strftime]: http://php.net/manual/fr/function.strftime.php "Documentation de la fonction strftime sur php.net"
[PHP_traits]: http://php.net/manual/fr/language.oop5.traits.php "Documentation des traits sur php.net"
[RFC_3986]: http://www.ietf.org/rfc/rfc3986.txt
[attributs]: #core-ref:bc3fad86-33cc-11e2-9a69-1bbd9c32b0f2
[type_attribut]: #core-ref:4e167170-33ed-11e2-8134-a7f43955d6f3
[attribut_calcule]: #core-ref:4565cab9-73c8-4eee-bfa7-218ffbd4b687
[aide_saisie]: #core-ref:0b2d4cd0-4eed-41d8-ac57-37525a444194
[contrainte]: #core-ref:7b41906b-f199-41a4-94df-33b9ad34153b
[definition_attribut]: #core-ref:bc3fad86-33cc-11e2-9a69-1bbd9c32b0f2
[workflow]: #core-ref:55a53d99-0c24-48d8-8cb9-1caa171f2e9a
[family_param]: #core-ref:4595c8e7-5002-4dbc-b6bb-882b4123efd8
[masque]: #core-ref:327ad491-06df-4e5b-b49a-695c75439fe1
[control_de_vue]: #core-ref:017f061a-7c12-42f8-aa9b-276cf706e7e0
[reset]: #core-ref:5c661733-772d-42b8-8b3e-b70453ddfd33
[importDocuments]: #core-ref:1c97f553-dcba-454e-96a0-8059230065b3
[phpfile]: {#core-ref:7362e2ff-cfb5-45f0-a81d-e02eab6d0fb6}
[def_enum]: #core-ref:eef3e3ec-2d50-41bd-98e1-cc978f0a5178
[visibility]:        #core-ref:3e67d45e-1fed-446d-82b5-ba941addc7e8
[getcustomtitle]:   #core-ref:3c5ff78d-c080-48fb-a293-9736ed4e95b8
[phpDocEmailRecipient]:     https://docs.anakeen.com/dynacase/3.2/dynacase-core-api-reference/interface_i_mail_recipient.html "PHPDoc : IMailRecipient"
[constraint]:       {#core-ref:f0177c62-1774-4724-a337-f090406e2d08}
[ErrorCodeATTR]: http://docs.anakeen.com/dynacase/3.2/dynacase-core-api-reference/class_error_code_a_t_t_r.html
[urlindex]:     #core-ref:9081464e-dfc9-4836-8577-cfa59829eaa0
[menuvis]:      #core-ref:0abcbf60-b1c5-4d78-b3a7-6574a369d99e
[wask]:         #core-ref:9e248e52-ad6b-4089-ab83-11a534b307e9
[phpfunc]:      #core-ref:1128e658-48f5-440f-9fd1-2d714e99eecd
[titleis]:      #core-ref:b0e414c0-b795-4bbe-b70e-a308b7f1b4ab