# Importation de documents par CSV {#core-ref:2fb3284a-2424-44b2-93ae-41dc3969e093}
 
Ce chapitre indique comme importer des documents au format
_[CSV][CSV]_ ou _[ODS][ODS]_.

## Constitution du fichier d'importation {#core-ref:31f163da-2878-43b9-9056-a17b63bdc114}

Le fichier d'importation peut être au format :

*   CSV
    *   Encodage : UTF-8,
    *   Séparateur de colonnes : `;`(point-virgule par défaut - configurable en <span class="flag from release inline">3.2.12</span>).
    *   Délimiteur de texte : ` ` (vide par défaut - configurable en <span class="flag from release inline">3.2.12</span>),
*   ODS OpenDocument Spreadsheet (tableur OpenOffice.org .ods)

Ce fichier d'importation est *auto-descriptif* ; c'est à dire qu'il

1.  définit l'[ordre et la signification des colonnes][order]
2.  liste les [documents à importer][doc]

### Ordre des attributs {#core-ref:e41116ee-a682-4033-a7ab-22dc1b99e56a}

Avant de décrire les valeurs des attributs d'un document il est nécessaire
d'indiquer à quel attribut correspond chaque valeur.

Pour définir l'ordre des attributs, il faut utiliser une ligne `ORDER`.  
Chaque ligne `ORDER` est propre à une famille (et ne concerne donc pas les
familles qui en héritent).

Cette ligne doit être présente avant les lignes décrivant les documents de la
famille concernée.

Les 4 premières colonnes contiennent : 

1.  1ère colonne : `ORDER`,
1.  2ème colonne : identifiant de la famille (nom logique)
1.  3ème colonne : vide (utilisée pour l'alignement avec la [ligne `DOC`][doc]),
1.  4ème colonne : vide (utilisée pour l'alignement avec la [ligne `DOC`][doc]).

Les autres colonnes définissent l'ordre des attributs à utiliser dans les
[lignes `DOC`][doc] de la même famille qui suivent.

Si plusieurs lignes `ORDER` référençant la même familles sont écrites à la suite,
les documents sont importés avec le dernier ordre interprété.

Exemple :

| #         | famille        |   |   |                   |                      |                      |
| --------- | -------------- | - | - | ----------------- | -------------------- | -------------------- |
| ORDER | ZOO\_CLASSE |   |   | cl\_latinname  | cl\_commonname    | cl\_caracteristic |
| DOC       | ZOO\_CLASSE     |   |   | Actinopterygii    | Poissons             | Nage                 |
| DOC       | ZOO\_CLASSE     |   |   | Reptilia          | Reptiles             | Rampe                |
|           |                |   |   |                   |                      |                      |
| ORDER | ZOO\_CLASSE |   |   | cl\_commonname | cl\_caracteristic | cl\_latinname     |
| DOC       | ZOO\_CLASSE     |   |   | Oiseaux           | Vole                 | Aves                 |
| DOC       | ZOO\_CLASSE     |   |   | Mammifères        | Allaite              | Mammalia             |

Dans ce cas, les deux premiers documents utilisent le premier `ORDER`, et les
documents suivants le deuxième `ORDER` (la deuxième ligne `ORDER` a écrasé les
directives de la première).

<span class="flag from release inline">3.2.19</span> Lorsque la ligne `ORDER`
est invalide (e.g. l'attribut déclaré n'existe pas pour la famille), alors
l'import de documents de cette famille est ignoré et une erreur est remontée.

La liste des codes d'erreur relatifs à la directive `ORDER` est consultable sur
la documentation PHP de la classe [ErrorCodeORDR][ErrorCodeORDR].

### Description d'un document {#core-ref:3acb8fbe-6e5a-4933-95fa-2cea0eae2fc5}

Les quatre premières ont une signification particulière :

1.  1ère colonne : toujours `DOC`,
1.  2ème colonne : identifiant de la famille (nom logique)

1.  3ème colonne : identifiant du document (numérique ou nom logique)
    
    *   s'il est saisi
        *   si le document référencé existe, il sera mis à jour,
        *   sinon, le document créé portera cet identifiant,
    *   sinon
        *   si l'option d'importation est à **_mise à jour_** (option par
            défaut) et qu'un document est trouvé au moyen des
            [clés d'importation][keys], il est mis à jour,
        *   sinon, un nouveau document est créé, et porte un nouvel identifiant
            généré par la base de données.
    
    <span class="flag nota-bene inline"/>Il n'est pas possible d'importer une révision 
    figée d'un document. Seules les révisions courantes peuvent faire l'objet 
    de modification. Si un identifiant pointe sur une révision passée 
    c'est la révision courante qui sera modifiée par l'importation.

1.  4ème colonne : identifiant du dossier dans lequel le document importé doit
    être inséré.
    
    *   Si un identificateur est précisé le document est référencé dans le
        dossier (s'il n'y est pas déjà).
    *   Si l'identificateur n'est pas précisé (vide ou zéro), le document est
        référencé dans le dossier courant (celui qui est sélectionné dans
        l'arborescence en cas d'utilisation avec l'interface web d'importation).
    *   Si l'identifiant est `-` alors le document ne sera référencé dans aucun
        dossier.

Les colonnes suivantes décrivent les valeurs **brutes** des attributs du
document.

## Mécanique d'importation {#core-ref:b9c8cc46-a3f2-4174-826b-f6456ad72613}

### Valeurs des attributs pour l'importation {#core-ref:2f4a65b8-9513-44e1-9002-bad13aea18ca}

Les valeurs inscrites dans le document sont insérés avec la
[méthode `Doc::setValue()`][setValue].

#### Relations {#core-ref:c8d7b2cf-6bb6-482e-a087-58f5c44dc97a}

Les attributs relations (`docid`, `account`, `thesaurus`) peuvent contenir :

*   soit le nom logique du document cible;
*   soit l'identifiant système du document cible.
    
    Attention, dans ce cas, le document cible peut ne pas être valable sur
    n'importe quelle instance de Dynacase.

Si le document référencé est un nom logique qui n'existe pas alors la valeur
sera ignorée (un message de warning est émis avec le nom logique qui n'est pas
connu). Si la référence est un nombre qui ne fait pas référence à un document,
ce nombre sera enregistré. Il sera considéré comme une référence inconnue.

#### Énumérés {#core-ref:e919dbd9-e70d-46f0-a91c-2fca281a91ae}

Les attributs de type énuméré (`enum`) doivent contenir la clef.

#### Dates {#core-ref:ea9b7493-3f51-4acd-9835-5d0469e44c0b}

Les dates (`date`, `timestamp`) doivent être renseignés au format ISO
(`YYYY-MM-DD` ou `YYYY-MM-DD HH:MM:SS` ou `YYYY-MM-DDTHH:MM:SS`).  
Dans le cas contraire, la date est interprétée en fonction de la locale de
l'utilisateur réalisant l'importation :

*   Si la locale de l'utilisateur est *français* (paramètre `CORE_LANG`
    positionné à la valeur `fr_FR`), alors la date est interprétée selon le
    format `DD/MM/YYYY` ;
*   Si la locale de l'utilisateur est *anglais* (paramètre `CORE_LANG`
    positionné à la valeur `en_US`), alors la date est interprétée selon le
    format `MM/DD/YYYY`.

Dans tous ces  cas la valeur de la date stockée sera au format ISO.

#### Attributs multivalués {#core-ref:1b8cd020-a2ed-4997-aefe-a4fcbb3564f1}

*   Pour les attributs multivalués à un seul niveau (attribut *contenu dans un
    tableau*, ou ayant l'option `multiple=yes` et n'étant pas contenu dans un
    tableau) :
    *   la chaîne `\n` délimite deux valeurs consécutives,
    *   la chaîne `<BR>` correspond à un saut de ligne au sein d'une valeur.
*   Pour les attributs multivalués à 2 niveaux (cas des relations ayant l'option
    `multiple=yes` et *contenues dans un tableau*) :
    *   la chaîne `\n` délimite deux valeurs de premier niveau,
    *   la chaîne `<BR>` délimite deux valeurs de second niveau.

Par exemple :

*   pour un attribut de type `longtext` contenu dans un tableau, la valeur
    `Première rangée<BR>suite du texte\nDeuxième rangée` correspond à la structure suivante :
    
    | index |              relations              |
    | :---: | ----------------------------------- |
    |   1   | Première rangée<br />suite du texte |
    |   2   | Deuxième rangée                     |
    
*   pour un attribut de type `docid`, ayant l'option `multiple=yes` et contenu
    dans un tableau, la valeur `1236\n6375<BR>8755<BR>564\n567<BR><BR>4569`
    correspond à la structure suivante :
    
    | index | relations                     |
    | :---: | ----------------------------- |
    | 1     | • 1236                        |
    | 2     | • 6375<br />• 8755<br />• 564 |
    | 3     | • 567<br />• <br />• 4569     |

**Note** <span class="flag from release">3.2.12</span> : Si un
délimiteur de texte est défini, le retour chariot est alors possible sur une
ligne et est interprété comme la chaîne `\n`.


### Hameçons déclenchés lors de l'importation {#core-ref:d3b06745-35c5-447c-9b88-01181736c21e}

Les [hameçons][hooks] sont lancés après la mise à jour des valeurs du document.

Lors d'un ajout de documents les hameçons suivants sont lancés :

1.  `Doc::preRefresh()`
1.  `Doc::postRefresh()`
1.  `Doc::preImport($extra)`
1.  `Doc::preCreated()`
1.  `Doc::postCreated()`
1.  `Doc::postStore()`
1.  `Doc::postImport($extra)`

Lors d'une mise à jour de documents les hameçons suivants sont lancés :

1.  `preRefresh()`
1.  `postRefresh()`
1.  `preImport($extra)`
1.  `postStore()`
1.  `postImport($extra)`

**Note** : Les contraintes ne sont pas vérifiées lors de l'importation des
documents. Le caractère obligatoire des attributs n'est pas vérifié non plus.

### Informations supplémentaires {#core-ref:15088f40-de05-4600-86bb-82422af01dce}

Lors de l'importation d'un document, il est possible d'indiquer des informations
supplémentaires pour réaliser des traitements conditionnels.

Ces informations sont identifiées par des clés commençant par `extra:` : ces
valeurs sont passées en paramètre des méthodes [`preImport`][preimport] et
[`postImport`][postimport] de la famille associée, sous forme d'un tableau
associatif de forme `clé => valeur`, avec pour clé la chaîne de caractères
suivant le `extra:` dans la description du nom de l'attribut, et comme valeur,
celle correspondante à la colonne.

Exemple :

| ORDER     | ZOO\_CLASSE |   |   | cl\_latinname   | extra:status | extra:special |
| --------- | ---------- | - | - | ------------   | ------------ | ------------- |
| DOC       | ZOO\_CLASSE |   | - | Actinopterygii | alive        | protected     |

Dans cette exemple, le document *Actinopterygii* de la famille *ZOO_CLASSE* aura
comme paramètre passé à `preImport` et `postImport` : 

    [php]
    array(
        'status' => 'alive',
        'special' => 'protected'
    );

### Clés d'importation {#core-ref:7eefc8e7-16a6-4188-99d5-c2c9d817a1fe}

Lors d'une importation, Dynacase utilise une heuristique pour déterminer si un
document existant doit être mis à jour ou s'il faut un créer un nouveau. Cette
heuristique se base sur les *clés d'importation* de la famille : si un et un
seul document de la même famille ou d'une sous-famille a ses clés d'importations 
égales à celles du document en cours d'importation, alors Dynacase fait une mise
à jour dans le cas contraire, Dynacase fait une création de document.

Les clés d'importation pour une famille sont déterminées au moyen d'une ligne
`KEYS`.  
Chaque ligne `KEYS` est propre à une famille (et ne concerne donc pas les
familles qui en héritent).

La clé utilisée par défaut pour reconnaître le document est la propriété `title`
de ce document.

L'ordre des colonnes est :

1. 1ère colonne : toujours `KEYS`,
1. 2ème colonne : Identifiant de la famille,
1. 3ème colonne : vide,
1. 4ème colonne : vide,
1. 5ème colonne : clef primaire,
1. 6ème colonne : clef secondaire.

Les colonnes 5 et 6 servent à définir les attributs ou propriétés qui sont utilisés comme
clé.  On peut avoir un maximum de deux clés par ligne (dans ce cas, il faudra
que les deux clés correspondent pour que le document soit reconnu).

Exemple :

|     #     |    famille     |     |     |                  |                   |                      |
| --------- | -------------- | --- | --- | ---------------- | ----------------- | -------------------- |
| __ORDER__ | __ZOO_CLASSE__ |     |     | __cl_latinname__ | __cl_commonname__ | __cl_caracteristic__ |
| __KEYS__  | __ZOO_CLASSE__ |     |     | __cl_latinname__ |                   |                      |
| DOC       | ZOO\_CLASSE     |     | -   | Actinopterygii   | Poissons          |                      |
| DOC       | ZOO\_CLASSE     |     | -   | Reptilia         | Reptiles          | Rampe                |
| DOC       | ZOO\_CLASSE     |     | -   | Actinopterygii   |                   | Nage                 |

Dans cet exemple, les documents sont reconnus par leur attribut
'cl\_latinname'. Donc pour le premier document, la clé est
'Actinopterygii', et pour le deuxième 'Reptilia'. Dans cet exemple le document 
_Actinopterygii_ est d'abord créé avec le nom _Poissons_, puis il est mis à jour 
avec la caractéristique _Nage_.

Les lignes `KEYS` sont appliquées dans l'ordre dans
lesquelles elles ont été écrites.

## Modifier l'icône d'un document {#core-ref:ac8ef5d6-fd4f-403e-bec1-486466f3f5f7}

Par défaut, l'icône d'un document est l'icône de la famille au moment de la
création du document.

Il est possible de modifier l'icône d'un document au moyen d'une ligne
`DOCICON`.

L'ordre des colonnes est  :

1. 1ère colonne : toujours `DOCICON`,
1. 2ème colonne : identifiant du document (nom logique),
1. 3ème colonne : nom du fichier image.

|     #     |    famille     |          |     |                  |                   |                      |
| --------- | -------------- | -------- | --- | ---------------- | ----------------- | -------------------- |
| __ORDER__ | __ZOO_CLASSE__ |          |     | __cl_latinname__ | __cl_commonname__ | __cl_caracteristic__ |
| DOC       | ZOO\_CLASSE     | CL\_ACTI  | -   | Actinopterygii   | Poissons          |                      |
| DOCICON   | CL\_ACTI        | fish.png | -   |                  |                   |                      |

Le nom de l'image doit référencer un fichier présent dans le répertoire
`./Images` à la racine du répertoire d'installation sur le serveur.


## Modifier un tag applicatif d'un document  {#core-ref:0ecabad3-6086-41fd-909c-9cfaa6f705dd}

<span class="flag from release inline">3.2.23</span>
Les tags applicatifs peuvent être ajoutés ou supprimés par le fichier d'importation *CSV*.

Il est possible de modifier le tag applicatif d'un document au moyen d'une ligne
`DOCATAG`.

L'ordre des colonnes est  :

1. 1ère colonne : toujours `DOCATAG`,
1. 2ème colonne : identifiant du document (nom logique),
1. 3ème colonne : non utilisée
1. 4ème colonne : action à réaliser : ADD, DELETE ou SET (par défaut ADD)
    1.  ADD : Ajout d'un nouveau tag
    2.  DELETE : Suppression du tag indiqué
    3.  SET : Suppression des précédents tag, puis ajout des nouveaux
1. Colonnes suivantes : autres tags à ajouter ou à supprimer

|      #      |      famille       |          |                |                   |                    |
| ----------- | ------------------ | -------- | -------------- | ----------------- | ------------------ |
| __ORDER__   | __ZOO_CLASSE__     |          |                | __cl\_latinname__ | __cl\_commonname__ |
| DOC         | ZOO\_CLASSE        | CL\_ACTI | -              | Actinopterygii    | Poissons           |
| DOC         | ZOO\_CLASSE        | CL\_AVES | -              | Aves              | Oiseau             |
| __#TAGKEY__ | __DOC IDENTIFIER__ |          | __TAG ACTION__ | __First Tag__     | __Second tag__     |
| DOCATAG     | CL\_ACTI           |          | ADD            | MyFishTag         |                    |
| DOCATAG     | CL\_AVES           |          | SET            | MyBirdTag         | MyOtherTag         |

Le tag ne doit pas contenir de retour chariot `\n`.

## Précautions d'utilisation {#core-ref:a3f0e390-b967-4de4-8bf8-85bedb173085}

Lors de l'import de documents quelques précautions d'usage sont à prendre en compte :

* Les attributs calculés peuvent devenir incohérent, ils sont en effet initialisés
avec les valeurs lors de l'import. Ceci peut créer des incohérences.
* Les documents de faisant référence à un `account` ne sont pas ré-importables tel
que. Il faut suivre la procédure de ré-import tel que décrite dans ce 
[chapitre][importation].

<span class="flag from release inline">3.2.18</span> Les documents ayant des
attributs en [visibilité][visibility] "I" ne sont pas importés depuis
l'interface d'importation sauf depuis le centre d'administration ou par [ligne
de commande][wshimport].

<!-- links -->
[CSV]: http://fr.wikipedia.org/wiki/Comma-separated_values "Comma-separated values sur wikipedia"
[ODS]: http://fr.wikipedia.org/wiki/OpenDocument "Open Document sur wikipedia"
[hooks]:            #core-ref:8f3d47de-32b5-4748-8a00-b1569c5423e5
[order]:            #core-ref:e41116ee-a682-4033-a7ab-22dc1b99e56a
[doc]:              #core-ref:3acb8fbe-6e5a-4933-95fa-2cea0eae2fc5
[setValue]:         #core-ref:febc397f-e629-4d47-955d-27cab8f4ed2f
[keys]:             #core-ref:7eefc8e7-16a6-4188-99d5-c2c9d817a1fe
[importation]:      #core-ref:a0cb9a84-6bde-476c-b55c-95c8f12abd3a
[preimport]:        #core-ref:adb6ba8b-15c4-42d3-97dc-1da16c2112ae
[postimport]:       #core-ref:9de7e922-150a-416b-b846-b6e195bf0921 
[wshimport]:        #core-ref:1c97f553-dcba-454e-96a0-8059230065b3
[visibility]:   #core-ref:3e67d45e-1fed-446d-82b5-ba941addc7e8
[ErrorCodeORDR] : http://docs.anakeen.com/dynacase/3.2/dynacase-core-api-reference/class_error_code_o_r_d_r.html
