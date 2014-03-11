# Description des tables de documents {#core-ref:b3ef653f-e8ca-4385-b551-2ead886133e6}

## La table _family.documents_ {#core-ref:d4b8d8ce-6f7a-4c1c-a5c4-f1adfcb74864}

Toutes les informations des documents sont enregistrées dans les tables héritées
de la table `family.documents`.

La table `family.documents` définit les [propriétés des documents][docprop] ;
c'est à dire tout ce qui est commun à toutes familles de documents.
 

|   Colonne    |             Type            |                                                                Définition                                                               |
| ------------ | --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| adate        | timestamp without time zone | Date de dernier accès au document.                                                                                                      |
| allocated    | integer                     | Identifiant système de l'utilisateur auquel le document est affecté.                                                                    |
| archiveid    | integer                     | Id de l'archive dans laquelle est contenu ce document.                                                                                  |
| atags        | text                        | Balises applicatives.                                                                                                                   |
| avalues      | json                        | Liste de l'ensemble des valeurs non nulles des attributs de données. (calculé par trigger) <span class="flag release from">3.3.0</span> |
| cdate        | timestamp without time zone | Date de création de la révision.                                                                                                        |
| confidential | integer                     | Indique si le document est confidentiel (>0).                                                                                           |
| cvid         | integer                     | Identifiant du document [contrôle de vue][CVDoc] associé à ce document.                                                                 |
| doctype      | character(1)                | Type de document.                                                                                                                       |
| domainid     | text                        | *pour application offline*                                                                                                              |
| dprofid      | integer                     | Identifiant du [profil dynamique][docprofdyn] associé à ce document.                                                                    |
| fromid       | integer                     | Id de la famille d'appartenance.                                                                                                        |
| fromname     | text                        | Nom logique de la famille d'appartenance.                                                                                               |
| id           | integer                     | Identifiant unique du document (issue de la séquence `family.seq_id_doc`).                                                              |
| icon         | text                        | Référence au fichier d'icone du document.                                                                                               |
| initid       | integer                     | Id du premier document de la lignée documentaire.                                                                                       |
| ldapdn       | text                        | *Obsolète* <span class="flag release obsolete">3.2.0</span>                                                                             |
| lmodify      | character(1)                | Tag de modification.                                                                                                                    |
| lockdomainid | integer                     | *Pour application offline*                                                                                                              |
| locked       | integer                     | Indique l'identifiant système de l'utilisateur qui a verrouillé le document.                                                            |
| name         | text                        | Nom logique du document.                                                                                                                |
| owner        | integer                     | Identifiant système de l'utilisateur ayant créé le document.                                                                            |
| postitid     | text                        | Identifiant du document post-it associé.                                                                                                |
| prelid       | integer                     | Identifiant du document (dossier) de relation primaire.                                                                                 |
| profid       | integer                     | Identifiant du [profil de document][docprof].                                                                                           |
| revdate      | integer                     | Date de révision.                                                                                                                       |
| revision     | integer                     | Numéro d'ordre du document dans sa lignée documentaire.                                                                                 |
| state        | text                        | Étape du document.                                                                                                                      |
| title        | character varying(256)      | Titre du document.                                                                                                                      |
| usefor       | text                        | Type d'utilisation du document.                                                                                                         |
| version      | text                        | Libellé de la version : il est vide par défaut.                                                                                         |
| views        | integer[]                   | Liste des [identifiants de compte][dbuser] qui ont accès au document.                                                                   |
| wid          | integer                     | Identifiant du [document *cycle de vie*][WF_document] associé à ce document.                                                            |


## Les tables des documents {#core-ref:0c6cc474-d5e9-4ee0-aeed-1aa00100d7df}

Pour chacune des familles, une table est créée. Cette
table  est nommée `family.<famname>` où `<famname>` est le nom logique de la
famille. 

Exemple :
![ Héritage des tables de documents ](advanced/dbinherit.png)

Les tables de documents sont créées avec le schéma _family_. 

Pour des raisons de compatibilité avec la version 3.2 de Dynacase, il est possible
de créer une vue avec l'identifiant numérique de la famille en
même temps que la table réelle. Pour plus de détail, reportez-vous au paragraphe
[sur le paramètre de compatiblité `CORE_DBDOCVIEWCOMPAT`](#core-ref:7bb6122b-ab0e-4d9f-8a67-2643d2369aa8).

**Important** :  À partir de Dynacase 3.3 (PostgreSQL 9.3), ces vues sont utilisables en mise à jour.

Note : Les noms des tables en postgresql ne sont pas sensibles à la casse.

La table `family.documents` ne comprend aucune donnée propre.

    [sql]
    db# SELECT count(id) FROM ONLY family.documents ; -- aucun document
     count 
    -------
         0
         
    db=# SELECT count(id) FROM family.documents ; -- tous les documents
     count 
    -------
     15008

Chacune des tables de documents contient en plus des propriétés héritées de la
table `family.documents`, une colonne par [attribut de données][attrdata].

Le nom de ces colonnes sont les identifiants des attributs. Le type de ces colonnes est
fonction du [type d'attribut][docattr]. Le type correspondant en base est
indiqué dans chacun des chapitres décrivant les attributs.

Pour plus de détails sur le mécanisme d'héritage de PostgreSQL :

- [PostgreSQL: Documentation: Inheritance][tutorial-inheritance]

## Table de recherche générale {#core-ref:378a7ed5-2703-4167-a980-c84bc29f8c56}

La table `public.docread` a les mêmes colonnes que la table `family.documents`
mais elle n'en hérite pas. Cette table contient une réplication de l'ensemble
des documents qui sont disséminés dans l'ensemble des tables `family.<famname>`.

Cette table répliquée a ses propres index globaux et permet dans le cas d'une
recherche globale, c'est à dire non liée à une famille particulière, d'être
plus rapide.

    [sql]
    db=# SELECT id, title FROM family.documents WHERE id = 1098;
      id  |           title           
    ------+---------------------------
     1098 | lecteur de Premier espace
    (1 ligne)
    Temps : 24,690 ms
    
    db=# SELECT id, title FROM docread WHERE id = 1098;
      id  |           title           
    ------+---------------------------
     1098 | lecteur de Premier espace
    (1 ligne)
    Temps : 1,037 ms


Cette table est maintenue synchronisée avec le contenu des tables
`family.<famname>` par des triggers, et ne doit donc être utilisée qu'en lecture
seule. Seules les tables `family.<famname>` peuvent être utilisées en
modification.

## Indexation des fichiers {#core-ref:354273d6-7fab-4096-8f22-6dc3e540ee65}

Si le [moteur de transformation][te] est opérationnel, les contenus des
fichiers peuvent être indexés. 


Un attribut fichier produit trois colonnes : une dans la table de sa famille et
deux autres dans la table du schéma `filecontent` associée à sa famille.

1.  colonne `<attrid>`
:   Contient la référence au fichier : `<vaultId>|<mimeType>|<fileName>`.
    
    1. vaultId : Identifiant dans le coffre (table `vaultdiskstorage`, colonne `id_file`),
    1. mimeType : type mime du fichier,
    1. fileName : nom du fichier (basename).

Colonnes de la table dans le schéma `filecontent` :

1.  colonne `<attrid>_txt`
:   Contient le contenu textuel (type `text`) du fichier qui a été retourné par 
    le moteur de transformation.
    
1.  colonne `<attrid>_vec`
:   Contient le contenu vectorisé (type `tsvector`) de la colonne `<attrid>_txt`.

## Recherche générale. {#core-ref:d5fc4bc8-067d-4dd8-b617-9641e3b11707}

Les tables du schéma `search` contiennet les valeurs recherchables des documents.


![ Tables de recherche ](advanced/dbsearch.png)


Ces tables sont utilisées 
pour les [recherches générales][globsearch].

Les colonnes `svalues` contient l'ensemble des valeurs [recherchables][attropt]
ainsi que les contenus des colonnes `_txt` des attributs fichiers présents dans
les tables du schéma `filecontent`.

Ces colonnes sont utilisées lors de recherche utilisant des expressions (partie de
mot).

Les colonnes `fulltext` contiennent la version vectorisée de `svalues`.
Cette colonne est utilisée pour les recherches globales de mot entier.

Ces deux colonnes sont calculées par trigger. Toutes modifications
d'enregistrements sur les tables documentaires déclenchent le recalcul de ces
colonnes.


<!-- links -->
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
[dbuser]:           #core-ref:6d5684f4-73e8-431c-8b2b-6224a9e6b074 "table users"
[tutorial-inheritance]: http://www.postgresql.org/docs/9.3/static/tutorial-inheritance.html
[attrdata]:         #core-ref:8a135f78-5d52-42e8-9601-a29c21ef98c9
[addgnlfilter]:     #core-ref:453cff11-09d9-4607-ab81-7acd36e99750