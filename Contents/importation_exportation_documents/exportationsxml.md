# Exportation de documents en _Xml_ {#core-ref:4d098747-0120-4ead-b13e-97ef1250e08d}

Les documents, hormis les familles, peuvent être exportés en XML. Les fichiers
XML produits suivent un schéma de famille imposé par dynacase. Chaque famille de
document a son propre schéma XML. L'exportation _Xml_ d'un contenu de dossier ou
de recherche peut se faire de deux façons : en générant un fichier par document
ou dans un seul fichier xml.

Si vous voulez utiliser le fichier _Xml_ pour une exportation vers un autre
contexte, il faut absolument indiquer l'option "sans les identificateurs"
, sinon les documents importés sont incohérents et peuvent corrompre le contexte.

Les fichiers produits par l'exportation _Xml_ sont conformes au format
d'[importation XML][importXML].

## Archive avec un fichier _Xml_ par document {#core-ref:1b0958b2-0788-45bf-974a-a7853a6030e2}

Dans ce cas une archive (zip) est constituée avec un fichier par document.
Chaque fichier est nommé à l'aide du titre et de l'identifiant du document (par
exemple : `alligator {4567}.xml`. Chaque fichier fait
référence à un [schéma xml][xmlschema] issus de la famille. Ce schéma est aussi
présent dans l'archive.

## Un fichier _Xml_ seul {#core-ref:9cebde3e-d40c-421c-85f6-e3e8282d547f}

Dans ce cas, la sortie est un seul fichier _xml_ contenant l'ensemble des
documents. La balise racine est dans ce cas `documents`. La balise documents
porte la date de génération et l'auteur.

    [xml]
    <documents date="2010-06-07T12:46:45"
    author="Default Master"
    name="recherche_animal">
        <zoo_animal .> </zoo_animal>
        <zoo_animal .> </zoo_animal>
    </documents>

Avec cette option les schémas _Xml_ des familles ne sont pas exportés.

## Format du XML de document {#core-ref:84e4a7f3-34f2-434e-bc32-54c4d1946cf3}

Les attributs non structurant sont insérés dans les attributs de structure (tab,
frame, array). 

    [xml]
    <my_tab><!-- Onglet -->
        <my_frame><!-- Cadre -->
            <my_array><!-- Tableau : rangée par rangée -->
                <my_attr_col1>Rangée 1 Colonne 1</my_attr_col1>
                <my_attr_col2>Rangée 1 Colonne 2</my_attr_col2>
            </my_array>
            <my_array>
                <my_attr_col1>Rangée 2 Colonne 1</my_attr_col1>
                <my_attr_col2>Rangée 2 Colonne 2</my_attr_col2>
            </my_array>
            
             <my_attribute1>Valeur</my_attribute1>
             <my_attribute2>Valeur</my_attribute2>
        </my_frame>
    </my_tab>

Les valeurs brutes des attributs non structurant sont contenus dans le
contenu de la balise.

### Attributs spécifiques {#core-ref:8818159b-2f2c-4b5e-9e0f-3a87d96c0d1b}

Les attributs de type ["docid"][docid] et ["file"][file] ont des attributs
supplémentaires.

#### Attribut XML docid, account, thésaurus {#core-ref:7b0f2e8f-bdd0-44b1-b664-2ab4e3975740}

*   `id` : Identifiant numérique de la relation
*   `name` : Nom logique de la relation
*   `revision` : <span class="flag from release">3.2.21</span> Contient 
    le numéro de la révision si l'option "[`docrev`][docrev]`=fixed`". 
    Contient "`state:<état>`" si l'option est "[`docrev`][docrev]`=state(<état>)`".

#### Attribut XML file, image {#core-ref:cdd3d492-f605-414c-9642-18391e9f441c}

*   `vid`: Identifiant du fichier dans le [coffre][vault]
*   `mime` : Type mime du fichier
*   `href` : Url de download du fichier
*   `title` : Nom du fichier

Le contenu de la balise est le fichier encodé si l'option "*Avec les fichiers*" a
été sélectionnée.

## Exemple d'un document exporté {#core-ref:f0d2fdee-efd0-4c31-937b-b29b7a5da5d2}

    [xml]
    <zoo_animal xsi:noNamespaceSchemaLocation="zoo_animal.xsd" id="3296" name=""
        title="Alli Alligator" revision="0" modification-date="2010-06-02T13:53:33"
        version="" state="">
        <an_tab1>
            <an_identification>
                <an_nom>Alli</an_nom>
                <an_descr xsi:nil="true"/>
                <an_tatouage xsi:nil="true"/>
                <an_espece_title>Alligator</an_espece_title>
                <an_espece id="1127">Alligator</an_espece>
                <an_ordre>Crocodiliens</an_ordre>
                <an_classe id="1123" name="Reptilia">Reptilia</an_classe>
                <an_sexe>M</an_sexe>
                <an_photo xsi:nil="true"/>
                <an_photo vid="27" mime="image/png" href="http://localhost:8080/dev/file/3296/743/an_photo/-1/Gator.png?cache=no&amp;inline=no" title="alligator.png"/>
                <an_naissance>2010-05-12</an_naissance>
                <an_entree>2010-05-18</an_entree>
                <an_pere id="1289">Viel Ali Alligator</an_pere>
            </an_identification>
        </an_tab1>
    </zoo_animal>

## Exportation _Xml_ avec fichier {#core-ref:b81787b0-8fca-4f05-97e6-dd163b3fc2aa}

Si vous cochez l'option avec les fichiers ceux-ci seront importés "inline" dans
les fichiers xml. La balise de l'attribut contiendra dans ce cas le fichier
encodé en base 64.

    [xml]
    <an_photo vid="27" mime="image/png" title="alligator.png">
    iVBORw0KGgoAAAANSUhEUgAAAQAAAAEACAYAAABccqhmAAAACXBIWXMAAAsTAAALEwEAmpwYAAAABGdBTUE
    AALGOfPtRkwAAACBjSFJNAAB6JQAAgIMAAPn/AACA6QAAdTAAAOpgAAA6mAAAF2+SX8VGAAEoA0lEQVR42m
    L8//8/wwgE==</an_photo>

## Exportation _Xml_ par ligne de commande {#core-ref:63238f3a-de09-45fa-b12b-854c569561c2}

L'exportation en ligne de commande est :

    ./wsh.php --app=FDL --action=EXPORTXMLFLD --id=[folderid] --eformat=[X|Y] --wfile=[N|Y]

Les paramètres sont :

`id`
:   Identifiant d'un dossier ou d'une recherche

`eformat`
:   Format d'export :
    
    *   X : Archive XML
    *   Y : _Xml_ simple

`wfile`
:   Avec ou sans les fichiers :
    
    *   Y : Avec fichiers
    *   N : Sans fichier

`flat`
:   Export *à plat*
    
    *   Y : Les balises de structure de type "tab" et "frame" ne sont pas
        représentées. Seule la structure concernant les tableaux est maintenue.
        
        Ce type d'exportation ne permet pas une réimportation.
        
    *   N : Export normal

`log`
:   Générer un fichier de log
    
    Cette option permet de connaître le nombre de documents exportées ainsi que
    leur liste.
    
        ./wsh.php --app=FDL --action=EXPORTXMLFLD --id=1180 --eformat=Y --log=/var/tmp/log.txt > t.xml
        $ more /var/tmp/log.txt
        EXPORT BEGIN OK : 02/07/2010 15:49:47
        EXPORT OPTION FLAT : no
        EXPORT OPTION WFILE : yes
        EXPORT OPTION CONFIG : yes
        EXPORT OPTION ID : ANIMAUX <Les animaux>
        EXPORT DOC OK : <a Agouti> [3300]
        EXPORT DOC OK : <Albert baleine blanche> [3266]
        EXPORT DOC OK : <Alli Baleine à bosse ou jubarte> [3248]
        EXPORT DOC OK : <Zoé Antilope> [3317]
        EXPORT COUNT OK : 3
        EXPORT END OK : 02/07/2010 15:49:47

`config`
:   Spécifie une liste de paramètres à exporter
    
    Cela est paramétrable à l'aide d'un fichier de configuration au format xml
    
    Exemple :
    
        configexport.xml
    
        [xml]
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>
            <family name="ZOO_ANIMAL">
                <attribute name="AN_NOM"/>
                <attribute name="AN_ESPECE"/>
                <attribute name="AN_ENFANT"/>
            </family>
                <family name="ZOO_ENCLOS">
                <attribute name="EN_NOM"/>
                <attribute name="EN_SURFACE"/>
            </family>
        </configuration>
    
        ./wsh.php --app=FDL --action=EXPORTXMLFLD --id=ANIMAUX --eformat=Y --config=configexport.xml > animaux.xml
    
    Chaque balise `family` définit les attributs à exporter pour cette famille.
    Seuls les attributs *feuille* non structurants sont pris en compte.
    
    Les attributs `name` désignent les identifiants de familles et d'attributs.
    
    Si on exporte un document d'une famille qui n'est pas dans le fichier de
    configuration, l'ensemble de ses attributs est exporté.


<!-- links -->
[xmlschema]: http://fr.wikipedia.org/wiki/XML_Schema
[importXML]:    #core-ref:81ad5a48-4c0f-468b-90ed-fe462fba7b96
[vault]:        #core-ref:4e91a88e-66a3-46e7-824d-d11adb0c39fe
[docid]:        #core-ref:d461d5f5-b635-47a0-944d-473c227587ab
[file]:         #core-ref:0e904376-317c-426e-bc6d-e56fd52bad89
[docrev]:       #core-ref:9bcfd205-fb07-4a71-be06-ba07d4a9cc7c