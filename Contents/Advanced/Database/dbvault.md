# Description des tables du coffre de fichiers {#core-ref:7c41b8d8-5ace-489c-886f-a6500c717423}

Ce chapitre définit les tables qui gèrent l'accès aux fichiers attachés aux
documents.

## Table `vaultdiskfsstorage` {#core-ref:15a90310-631d-4c9f-9b27-a505b24bfe94}

La table `vaultdiskfsstorage` indique les différents coffres enregistrés.


|      Colonne       |           Type          |                                 Définition                                  |
| ------------------ | ----------------------- | --------------------------------------------------------------------------- |
| id_fs              | integer                 | Identifiant de coffre (issue de la séquence `seq_id_vaultdiskfsstorage`)    |
| fsname             | text                    | Nom du coffre                                                               |
| max_size           | bigint                  | Capacité maximum du coffre (en octets)                                      |
| free_size          | bigint                  | Capacité restante en octets (calculé en fonction de la taille des fichiers) |
| subdir_cnt_bydir   | integer                 | *Obsolète* Non utilisé                                                      |
| subdir_deep        | integer                 | *Obsolète* Non utilisé                                                      |
| max_entries_by_dir | integer                 | *Obsolète* Non utilisé                                                      |
| r_path             | text                    | Chemin absolu d'accès au coffre                                             |

Le répertoire `r_path` doit être accessible en lecture/écriture à l'utilisateur
du serveur web.

## Table `vaultdiskdirstorage` {#core-ref:88e4d061-5812-4cec-8e30-cd0b3ea1c8cd}

La table `vaultdiskdirstorage` indique les différents répertoires utilisés par
les coffres.

Chaque répertoire peut contenir jusqu'à 1000 fichiers.

À chaque ajout d'un fichier dans un répertoire, son `free_entries` est
décrémenté de 1.

Lorsque le `free_entries` d'un répertoire arrive à 0, alors un nouveau
répertoire est créé, et sa capacité nominale (`free_entries`) repart à
1000.

Chaque répertoire ne peut avoir que 100 sous-répertoires. Lorsque tous les
répertoires d'un même niveau sont pleins, alors un nouveau répertoire de
niveau inférieur est créé.


|   Colonne    |           Type          |                                    Définition                                   |
| ------------ | ----------------------- | ------------------------------------------------------------------------------- |
| id_dir       | integer                 | Identificateur de répertoire (issue de la séquence `seq_id_vaultdiskdirstorage) |
| id_fs        | integer                 | Identificateur du coffre lié au répertoire                                      |
| free_entries | integer                 | Nombre d'entrée libre (1000 à l'initialisation)                                 |
| l_path       | text                    | Chemin du répertoire relatif à `r_path`                                         |



## Table `vaultdiskstorage` {#core-ref:4e91a88e-66a3-46e7-824d-d11adb0c39fe}

La table `vaultdiskstorage` stocke les chemins d'accès et les propriétés des
fichiers du coffre.

Les fichiers physiques sont nommés `<id_file>.<extension>`.

Si _id_file=342_ et _name='mon test.pdf'_ alors le nom physique est `342.pdf`.

Le fichier est placé dans le répertoire `r_path/l_path`. `r_path` provient du
coffre lié (`id_fs`) et `l_path` provient du répertoire lié (`id_dir`).

|    Colonne    |             Type            |                                           Définition                                          |
| ------------- | --------------------------- | --------------------------------------------------------------------------------------------- |
| id_file       | integer                     | Identificateur de fichier (issue de la séquence `seq_id_vaultdiskstorage)                     |
| id_fs         | integer                     | Identificateur du coffre lié au fichier                                                       |
| id_dir        | integer                     | Identificateur du répertoire lié au fichier                                                   |
| public_access | boolean                     | Si vrai, l'[accès au fichier][expfile] est possible sans contrôle du droit d'accès au document |
| size          | integer                     | Taille du fichier (en octets)                                                                 |
| name          | text                        | Nom du fichier original (basename)                                                            |
| mime_t        | text                        | Type mime (humainement lisible, e.g. `PDF document, version 1.4`)                             |
| mime_s        | text                        | Type mime système (e.g. `application/pdf`)                                                    |
| cdate         | timestamp without time zone | Date d'insertion dans le coffre                                                               |
| mdate         | timestamp without time zone | Date de modification dans le coffre                                                           |
| adate         | timestamp without time zone | Date de dernier accès depuis le coffre                                                        |
| teng_state    | integer                     | État du fichier converti                                                                      |
| teng_lname    | text                        | Nom du fichier converti                                                                       |
| teng_id_file  | integer                     | Identifiant du fichier converti                                                               |
| teng_comment  | text                        | Message de conversion                                                                         |

Les attributs de document de type [`file`][attrfile] et [`image`][attrimg]
référencent l'identifiant du fichier. La valeur d'un tel attribut dans un
document est : `<Type Mime>|<Identifiant Fichier>|<Nom du fichier>`.

L'_identifiant fichier_ référence la colonne `id_file` de la table
`vaultdiskstorage`.

![ Relations entre les tables des coffres de fichiers ](advanced/dbvault.png)



<!-- links -->
[appact]:           #core-ref:b26f57fe-4d75-4d5f-a50e-129028b379ed "Applications et action"
[app]:              #core-ref:395f44f1-6699-4ad8-b525-31e65e9b6efb "Écrire une application"
[action]:           #core-ref:e67d8aeb-939c-46e3-9be8-6fc3ba75ebc2 "Écrire une action"
[openaccess]:       #core-ref:9edc8f2e-6929-11e2-8610-0021e9fffec1 "Authentification par jetons"
[dbacl]:            #core-ref:c7caa985-3b34-4abd-8ffa-2e7110718efc "Table acl"
[acls]:             #core-ref:a98b72ea-c063-4907-abc4-e5171ab55e59 "Les droits applicatifs"
[expfile]:          #core-ref:0fc93676-888f-497e-b9bc-28cdbead52df "Action EXPORTFILE"
[attrfile]:         #core-ref:0e904376-317c-426e-bc6d-e56fd52bad89 "Attribut file"
[attrimg]:          #core-ref:4fca7712-59e0-4186-bfd0-6214104a0f60 "Attribut image"
 