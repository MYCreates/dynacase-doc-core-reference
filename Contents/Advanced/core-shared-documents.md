# Utilisation des documents partagés du noyau {#core-ref:947948f6-242c-40a7-8f70-8013fe2ab1f1}

## Introduction  {#core-ref:fd8e5ba1-0e9e-4614-b592-eaeb46d8c6cb}

Dynacase core utilise un objet de partage de documents lors de l'exécution des
actions. Cet objet de partage permet d'adresser le même objet document lorsque
la fonction [`new_doc`][new_doc] est appelée plusieurs fois pour récupérer le même document.

Le noyau utilise cette fonction pour ses besoins internes et le peuplement de
cet objet de partage est fait pour répondre au besoin du noyau.

Cependant, lorsqu'on utilise la fonction [`new_doc`][new_doc], l'objet document
récupéré peut être celui du partage ou un nouvel objet peuplé avec les valeurs
de la base de données.

Seule la fonction [`new_doc`][new_doc] utilise la fonction de partage pour
récupérer des documents de cet objet de partage.  Cette fonction récupère le
document dans l'objet de partage s'il a été enregistré avec son identifiant. Si
le document n'est pas dans cet objet de partage, un nouvel objet document est
créé avec les données de la base de données et il est ajouté à l'objet de
partage si la [limite][limite] n'est pas atteinte.

Le document n'est pas récupéré de la zone de partage par la fonction
[`new_doc`][new_doc] si :

*   le document est un[ cycle de vie][workflow] instancié 
*   le document est un [profil dynamique][dynprof] 
    (ceci inclus les contrôles de vue avec profilage dynamique)

## Cas de modification de l'objet de partage  {#core-ref:a3c838cd-56a3-4016-a055-960736bc2806}

Le noyau modifie cet objet de partage dans les fonctions suivantes :

`new_doc()`
:   Cette fonction ajoute le document dans le partage s'il n'y est pas déjà et
     si le document n'est pas complet (seulement en cas d'installation de famille)

`Doc::postInsert()` 
:   Cette méthode ajoute le document si ce n'est pas une famille.  
    La méthode `Doc::postInsert` est appelée
    à chaque création ([`Doc::store()`][docstore]) d'un nouveau document en base 
    et à chaque enregistrement d'une nouvelle révision.  
    
    Note : avec un [cycle de vie][workflow], une révision est créée à chaque 
    passage de transition.
    Une révision constitue une nouvelle entrée mais n'efface pas l'ancienne.

`Doc::convert()` 
:    Ceci constitue une utilisation moins commune qui consiste à convertir un 
    document d'une famille à une autre. Dans ce cas, le document partagé s'il existe
    est mis à jour avec le résultat de la conversion.


## Limite de stockage {#core-ref:06221289-ad24-421f-8daa-84ebeaa677cd}

Cet objet de partage est limité en nombre d'éléments à garder en mémoire. 
Si la limite est atteinte aucun nouvel ajout dans le partage n'est effectué.

Par défaut la limite est de 20 (_define_ `MAXGDOCS`).

## Méthodes de gestion du partage de document {#core-ref:20377278-9118-471c-96f1-29866641b0cf}

|    Méthode <span class="flag from release">3.2.21</span>    |                                                            Description                                                             |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `Doc  Dcp\Core\SharedDocuments::get(int $id)`               | Récupère un document de l'objet de partage référencé par son identifiant (propriété `id`). Retourne `null` si inexistant           |
| `bool Dcp\Core\SharedDocuments::set(int $id,Doc $doc)`      | Ajoute ou remplace un document, la clef de partage est l'identifiant du document. Retourne `false` si non ajouté (limite atteinte) |
| `bool Dcp\Core\SharedDocuments::exists(int $id)`            | Vérifie que la clef de partage existe                                                                                              |
| `bool Dcp\Core\SharedDocuments::remove(int $id)`            | Supprimer le partage d'un document                                                                                                 |
| `bool Dcp\Core\SharedDocuments::clear()`                    | Enlève le partage de tous les documents partagés                                                                                   |
| `int  Dcp\Core\SharedDocuments::getLimit()`                 | Retourne la limite de nombre de document partagé                                                                                   |
| `void Dcp\Core\SharedDocuments::setLimit(int $limit)`       | Modifie la limite de nombre de document partagé                                                                                    |
| `bool Dcp\Core\SharedDocuments::isShared(int $id,Doc $doc)` | Vérifie si le document référencé par son identifiant est dans le partage                                                           |



## Exemple {#core-ref:2c2ae7d9-8eac-4798-86a1-da8a00c8e2ba}

Pour mieux comprendre, la constitution de ce partage et son utilisation, l'exemple
suivant va illustrer ce partage.
L'exemple présuppose que la limite n'est jamais atteinte.

---------------------------------------
`$docX=new_doc("", 1234)`

| Clefs | Objets |
| ----- | ------ |
|  1234 | $docX  |

Ajout de la clef 1234.


---------------------------------------
`$docY=new_doc("", 2345)`:

| Clefs | Objets |
| ----- | ------ |
|  1234 | $docX  |
|  2345 | $docY  |

Ajout de la clef 2345.

---------------------------------------
`$docW=createDoc("","MY_FAMILY");`

| Clefs | Objets |
| ----- | ------ |
|  1234 | $docX  |
|  2345 | $docY  |

Rien de modifié. La méthode Doc::Store() n'est pas encore appelée

---------------------------------------

`$docW->store()`// acquisition d'un nouvel identifiant 5688

| Clefs | Objets |
| ----- | ------ |
|  1234 | $docX  |
|  2345 | $docY  |
|  5688 | $docW  |

Ajout de la clef 5688 via le Doc::postInsert() appelé depuis le Doc::store()

---------------------------------------
`$docZ=new_doc("", 1234)`:

| Clefs |    Objets    |
| ----- | ------------ |
|  1234 | $docX, $docZ |
|  2345 | $docY        |
|  5688 | $docW        |

La variable $docZ récupère la clef de partage 1234. Maintenant $docX === $docZ.

---------------------------------------
    print $docX->getValue("x"); // print 21 (on suppose que 21 est la valeur initiale de ce document)
    $docZ->setValue("x", 3);
    print $docZ->getValue("x"); // print 3
    print $docX->getValue("x"); // print 3
    $docZ->store();

| Clefs |    Objets    |
| ----- | ------------ |
|  1234 | $docX, $docZ |
|  2345 | $docY        |
|  5688 | $docW        |

Aucune modification. Le Doc::store() ne fait pas de création.

---------------------------------------
`$docZ->revise();` // acquisition d'un nouvel identifiant 5689

| Clefs |    Objets    |
| ----- | ------------ |
|  1234 | $docX, $docZ |
|  2345 | $docY        |
|  5688 | $docW        |
|  5689 | $docX, $docZ |

La révision créé un nouvel identifiant. Le Doc::postInsert() qui est appelé via
le Doc::revise() ajoute une nouvelle entrée 5689. L'entrée 1234 n'est pas
enlevée.


---------------------------------------
    print $docX->id; // print 5689
    $docX->setValue("x", 52);
    print $docZ->getValue("x"); // print 52
    
    $docL=new_doc("", 1234, true); // Dernière révision => 5689
    print $docL->getValue("x"); // print 52

| Clefs |        Objets       |
| ----- | ------------------- |
|  1234 | $docX, $docZ, $docL |
|  2345 | $docY               |
|  5688 | $docW               |
|  5689 | $docX, $docZ, $docL |

L'appel `new_doc("", 1234, true`) est équivalent à l'appel `new_doc("", 5689)`.
La variable $docL contient le document partagé avec la clef 5689.
Maintenant $docX === $docZ === $docL.

---------------------------------------
    $docX=new_doc("", 1234);
    print $docX->getValue("x"); // print 3 (valeur de la révision précédente)

| Clefs |    Objets   |
| ----- | ----------- |
|  1234 | $docX       |
|  2345 | $docY       |
|  5688 | $docW       |
|  5689 | $docZ $docL |

La fonction "new_doc" remplace la référence au partage car la clef a changé.
Cet appel a demandé une révision ancienne du document.


<!-- links -->
[new_doc]:   #core-ref:e978cbd1-5f54-4a06-a6be-f1c079c2d734
[docstore]:  #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[limite]:    #core-ref:06221289-ad24-421f-8daa-84ebeaa677cd
[workflow]:   #core-ref:932119d9-2681-427f-bcf2-2c439784d051
[dynprof]:    #core-ref:bc24834a-b380-4681-ae94-08b93076a7e8