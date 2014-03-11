# Description de la table des familles de document {#core-ref:ccad55c7-5ff1-40c3-ad6f-ec79b1a9ac3c}

La table `family.families` contient les caractéristiques des [familles][famprop]. Cette
table hérite de la [table `family.documents`][dbdoc].

Colonnes spécifiques de la table `family.families` :

|    Colonne    |       Type       |                                          Définition                                         |
| ------------- | ---------------- | ------------------------------------------------------------------------------------------- |
| cprofid       | integer          | Indique le [profil][docprof] utilisé pour les documents créé de cette famille               |
| dfldid        | integer          | Identifiant du dossier principal                                                            |
| cfldid        | integer          | Identifiant de la recherche principale                                                      |
| ccvid         | integer          | Indique le [contrôle de vue][CVDoc] qui sera associé aux documents créés avec cette famille |
| classname     | text             | Nom de la classe PHP associée au document (utilisé dans la table `family.families` uniquement). |
| methods       | text             | Liste des fichiers méthodes (METHOD)                                                        |
| defval        | text             | Valeurs par défaut                                                                          |
| schar         | character(1)     | Indique les modalités de révision                                                           |
| param         | text             | Liste des valeurs des paramètres                                                            |
| maxrev        | integer          | Nombre [maximum de révisions][docrevise] pour les documents de cette famille                |
| configuration | text             | Stockage des caractéristiques des propriétés (voir [Définition de paramètres de propriété][import_prop]) |
| tagable       | text             | Spécifique module [Dynacase tags][tag]                                                      |


![ Relation d'héritage entre family.families et family.documents ](advanced/dbdocfam.png)

<!-- links -->
[famprop]:          #core-ref:cfc7f53b-7982-431e-a04b-7b54eddf4a75  "Propriétés des familles"
[dbdoc]:            #core-ref:b3ef653f-e8ca-4385-b551-2ead886133e6 "Description de la table doc"
[docprop]:          #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353 "Propriétés de la classe Doc" 
[docrevise]:        #core-ref:882e3730-0483-4dbc-9b9d-0d0b5cc31d38 
[docgetprop]:       #core-ref:80e3ca49-26ea-4090-9fcd-7b92bf9d5c6d
[CVDoc]:            #core-ref:017f061a-7c12-42f8-aa9b-276cf706e7e0
[profil_dynamique]: #core-ref:bc24834a-b380-4681-ae94-08b93076a7e8
[wsh_cleanContext]: #core-ref:100b123b-da1a-45b4-848b-0622f3e09a40
[WF_document]:      #core-ref:b541e22f-5ece-4d19-8460-0cb0c5f3ec7a
[docprof]:          #core-ref:f1575705-10e8-4bf2-83b3-4c0b5bfb77cf
[docprofdyn]:       #core-ref:bc24834a-b380-4681-ae94-08b93076a7e8
[attropt]:          #core-ref:16e19c90-3233-11e2-a58f-6b135c3a2496 "option searchcriteria"
[destroyfam]:       #core-ref:534bf934-5a2f-4160-8912-567d8f933ddb "wsh destroyFamily"
[docattr]:          #core-ref:4e167170-33ed-11e2-8134-a7f43955d6f3 "Description des types d'attribut"
[te]:               https://docs.anakeen.com/dynacase/3.2/dynacase-doc-tengine-installation-operating/website/book/index.html "Manuel d'installation"
[globsearch]:       #core-ref:19b9f4b4-c960-46eb-b4e0-805ed76be3a6
[tag]:              https://docs.anakeen.com/dynacase/3.2/dynacase-doc-tags/website/book/index.html "Module Dynacase tags"
[import_prop]:      #core-ref:40d229c4-33c4-11e2-9147-a3eaf356c37c