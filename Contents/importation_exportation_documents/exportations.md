# Exportation de documents {#core-ref:88fb91b5-51a3-4b33-ac2e-5f20eddd8210}
 
L'accès à l'interface d'exportation se fait depuis le _centre d'administration_.
avec l'application _Explorateur de documents_.

![ Exportation de documents ](ihmexportation.png)

Pour exporter, il faut cliquer sur le menu _Outils/Exportation_ du dossier
depuis la fenêtre d'ouverture de dossier (ou de recherche). L'accès à
l'exportation est aussi disponible sur le menu contextuel affichable en cliquant
sur les icônes de dossier ou de recherche présentes dans l'arborescence.

<span class="flag from release inline">3.2.18</span> Les documents ayant des
attributs en [visibilité][visibility] `I` sont exportés uniquement si cette
interface est utilisée depuis le centre d'administration. Si l'interface est
utilisée hors de ce contexte, les valeurs des attributs en visibilité `I` ne
sont pas exportés.

## Options d'exportation {#core-ref:83ec5f8c-c048-4da9-ab81-edd5d52efc0d}

L'exportation propose les options suivantes :

Encodage 
:   Permet de spécifier l'encodage du fichier d'exportation.
    
    *   `ISO 8859-15` : est utile pour les systèmes ne gérant pas correctement
        l'UTF8 (cas par exemple d'Excel sous Windows)
    *   `UTF-8` : valeur préférée lorsque le système gère correctement cet
        encodage.
    
    Lors d'une [importation][ImportationCSV], l'encodage est détecté
    automatiquement et ces 2 encodages sont supportés.

Profil
:   Permet d'exporter les profils associés aux documents.
    
    Si des profils sont liés à des documents, le document profil et son
    profilage sont aussi exportés.
    
    Cette option est utile lorsqu'un concepteur de famille veut récupérer son
    [paramétrage profil][exportprofil].

Fichiers
:   Permet d'exporter également les fichiers attachés aux documents.
    
    *   `sans les fichiers` : indique que seul le fichier _csv_ contenant les
        valeurs des attributs sera exporté. Les fichiers liés aux attributs de
        type [file][attrfile] ou [image][attrimage] ne sont pas exportés.
    *   `avec les fichiers` : indique que l'exportation doit générer un fichier
        archive de type Zip. Cette archive contiendra
        *   le fichier `fdl.csv`, contenant les valeurs des attributs
        *   les fichiers attachées aux documents exportés.
        
        Cette archive peut être réimporté à au moyen de
        l'[importation d'archive][importArchive].
        
        Pour des raisons de compatibilité avec les différents systèmes
        d'exploitations les noms des fichiers exportés ne comportent pas
        d'accents.

Identificateur
:   Permet de sauvegarder les identificateurs systèmes numériques des documents.
    
    *   `Avec les identificateurs` : Cela implique que votre fichier n'est
        ré-importable que dans la même base. En contrepartie, les documents
        sont identifiés de manière non ambiguë dans l'objectif d'une restauration.
    *   `Sans les identificateurs` : permet de réimporter les documents dans
        n'importe quelle base. Cela implique que lors d'une réimportation dans
        la même base, la mise à jour des documents existants n'est pas garantie
        (voir pour cela la notion de [Clés d'importation][keys]).
    
    Dans tous les cas, si les documents comportent des noms logiques (propriété
    `name`), ces noms sont exportés à la place des identifiants systèmes.

Choisir les colonnes
:   Permet de n'exporter que certains attributs du documents.
    
    Chaque administrateur  peut définir et sauvegarder les attributs qu'il veut
    exporter pour chaque famille.
    
    L'interface de choix des colonnes est présentée lorsqu'une famille est
    sélectionnée. Une fois le choix sélectionné et sauvegardé alors seuls les
    attributs choisis seront exportés. Le  choix _"Ne pas tenir compte des
    préférences"_ permet d'exporter l'ensemble des attributs.

Format
:   Voir le [chapitre suivant][format]

Délimiteur de texte  <span class="flag from release">3.2.17</span>
:   Permet de configurer le format du CSV produit (uniquement dans le cas d'un format non XML)
    
    Ce paramètre permet de définir le délimiteur de cellule (enclosure) du CSV produit. Le défaut de 
    Dynacase est `Aucun délimiteur` (chaîne vide), le standard du CSV est `"`.

Séparateur de cellule  <span class="flag from release">3.2.17</span>
:   Permet de configurer le format du CSV produit (uniquement dans le cas d'un format non XML)
    
    Ce paramètre permet de définir le séparateur de cellule du CSV produit. Le défaut de Dynacase est `;`, le standard
    CSV est `,`.

**Attention** : A chaque utilisation de l'exportation de documents, le délimiteur de texte et le séparateur de cellule
utilisés sont sauvés automatiquement pour l'utilisateur en cours et utilisés pour les exportations suivantes.

## Différents formats d'exportation {#core-ref:e51257aa-3a16-41be-94ed-7ef2206a899b}
  
### Données réimportables {#core-ref:aa0070b2-5d64-4882-b083-acd05450059a}

Le fichier produit est réimportable. Son format dépend de l'option `Fichiers` :

*   Si l'option _Sans les fichiers_ est choisie, le fichier produit est un
    fichier _csv_ conforme au format d'[importation CSV][ImportationCSV].
    
    Exemple de fichier _csv_ produit : (ici avec l'option _avec les identificateurs_)
    
    | //FAM | animal(ZOO_ANIMAL) | Identifiant | Dossier |   nom    |    espèce    |   classe  |
    | ----- | ------------------ | ----------- | ------- | -------- | ------------ | --------- |
    | ORDER | ZOO_ANIMAL         |             |         | an_nom   | an_espece    | an_classe |
    | DOC   | ZOO_ANIMAL         | aliRotor    |         | Rotor    | ZOO_ESP_ALLI | Reptilia  |
    | DOC   | ZOO_ANIMAL         | 2079        |         | Théodor  | ZOO_ESP_ALLI | Reptilia  |
    | DOC   | ZOO_ANIMAL         | 2080        |         | Éléonore | ZOO_ESP_ALLI | Reptilia  |

*   Si l'option _avec les fichiers_ est choisie, le fichier produit est une
    archive de type zip conforme au format
    d'[importation d'archive][importArchive]

### Données brutes {#core-ref:2e22b0a8-5148-4f74-8cc8-e56baae42c7e}

Le fichier produit est un fichier _csv_. ce fichier n'est pas réimportable.
Il ne contient que les valeurs brutes d'attributs.

Pour les relations c'est le nom logique qui est exporté s'il existe, sinon c'est
l'identifiant système.

|   nom   |    espèce    |  classe  |
| ------- | ------------ | -------- |
| Rotor   | ZOO_ESP_ALLI | Reptilia |
| Théodor | ZOO_ESP_ALLI | Reptilia |
| Bouti   | 2456         | Mammalia |
| Germiné | 2476         | 1678     |

La première ligne contient les libellés des attributs exportés. Ces libellés
peuvent différer d'une locale à une autre si des traductions ont été
enregistrées. Dans ce cas, ce sont les libellés correspondant à la locale de
l'utilisateur qui a fait l'exportation.

### Données formatées {#core-ref:538a9069-167f-43de-8a25-96d0dd5d70e9}

 Le fichier produit est un fichier _csv_. Ce fichier n'est pas réimportable.
Il ne contient que les valeurs formatées d'attributs :

*   Les relations sont remplacées par le titre du document cible à la place de
    son identifiant,
*   Les énumérés sont remplacés par leur libellé à la place de la clé.
*   Les attribut de type `money` sont affichés avec séparation des milliers.

À la différence des données brutes, l'espèce contient le titre et non
l'identifiant. De même, les énumérés contiendront le libellé et non la clefs. La
valeur formatée exportée est celle rendue sur les interfaces web à l'exception
des hyperliens. Les éventuelles formats d'affichages sont pris en compte comme par
exemple `double("%.03f %%")` qui donnera `2.000 %` au lieu de `2`.

|   nom   |      espèce     |   classe  |
| ------- | --------------- | --------- |
| Rotor   | Alligator       | Reptile   |
| Théodor | Alligator       | Reptile   |
| Bouti   | Agouti          | Mammifère |
| Germiné | Caille des blés | Oiseau    |

La première ligne contient les libellés des attributs exportés.

### Données XML Zip {#core-ref:dff8dc78-0ab8-481d-9e44-3a5cb0de2c61}

Chaque document est enregistré dans un fichier XML. Ces fichiers sont insérés
dans un fichier d'[archive Zip][exportxmlzip].

### Données XML simple {#core-ref:f600301c-c2c3-4097-a4d7-1e57b8477dbc}

Tous les documents sont enregistrés dans un seul et même [fichier
XML][exportxml].

## Exportations et révisions {#core-ref:c44857d2-6a33-4552-af61-3aafad359416}

Les documents révisés peuvent faire partie du résultat de l'exportation si la
recherche spécifie l'option "toutes les révisions".

Si un document révisé (document figé) à un nom logique, il ne sera pas utilisé
comme [identificateur][importcsvid].  C'est son identifiant numérique qui sera
utilisé dans ce cas.

Si un document référence une révision précise d'un document (option `docrev`
différente de "latest"), c'est son identifiant numérique qui sera utilisé même
s'il possède un nom logique. Dans ce cas, un avertissement sera affiché.

<span class="flag nota-bene inline"/>La réimportation d'une exportation qui
comporte des révisions va créer de nouveaux documents si les identifiants
n'existent pas déjà ou mettre à jour la révision courante avec les valeurs de
révisions précédentes.


<!-- links -->
[ImportationCSV]: #core-ref:2fb3284a-2424-44b2-93ae-41dc3969e093
[importArchive]: #core-ref:021b7db1-7baf-48c4-8eb9-4a388355dd86
[attrfile]:  #core-ref:0e904376-317c-426e-bc6d-e56fd52bad89
[attrimage]: #core-ref:4fca7712-59e0-4186-bfd0-6214104a0f60
[exportxmlzip]: #core-ref:1b0958b2-0788-45bf-974a-a7853a6030e2 "détail format exportation archive XML"
[exportxml]: #core-ref:9cebde3e-d40c-421c-85f6-e3e8282d547f "détail format exportation XML"
[exportprofil]: #core-ref:602c6331-7cdb-4b24-8a56-ffd11e00502f
[keys]: #core-ref:7eefc8e7-16a6-4188-99d5-c2c9d817a1fe
[format]: #core-ref:e51257aa-3a16-41be-94ed-7ef2206a899b
[visibility]:   #core-ref:3e67d45e-1fed-446d-82b5-ba941addc7e8
[importcsvid]:  #core-ref:3acb8fbe-6e5a-4933-95fa-2cea0eae2fc5
[docrev]:       #core-ref:9bcfd205-fb07-4a71-be06-ba07d4a9cc7c
