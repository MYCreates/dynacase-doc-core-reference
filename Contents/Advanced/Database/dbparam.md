# Description des tables de paramètres applicatif {#core-ref:1c9a8c60-f102-4ff3-951b-a6faefcaf99f}

Ce chapitre décrit les tables qui définissent où sont enregistrés les
définitions et les valeurs des [paramètres applicatifs][paramapp].



## Table `paramv` {#core-ref:a4109cab-2551-4221-a112-93c909577344}

La table `paramv` contient les valeurs des paramètres applicatifs. Elle contient
notamment les valeurs des [paramètres applicatifs de référence][paramref].

Les valeurs des paramètres sont gérés à travers la classe de [Gestion des
paramètres][parammgr].


| Colonne |          Type         |                                    Définition                              |
| ------- | --------------------- | -------------------------------------------------------------------------- |
| name    | text                  | Nom du paramètre                                                           |
| type    | text                  | Type du paramètre : `G` (global), `A` (applicatif), `U<uid>` (utilisateur) |
| appid   | integer               | Identifiant de l'application                                               |
| val     | text                  | Valeur du paramètre                                                        |

Un paramètre, s'il est _global_ et non utilisateur, ne peut avoir qu'un seule
valeur.

Si le paramètre est _applicatif_ et non utilisateur, il ne peut avoir qu'une
seule valeur par application.

Si le paramètre est utilisateur, il peut avoir une valeur par utilisateur.
Dans ce cas le type est composé du caractère `U` suivi de l'identifiant de
l'utilisateur (`id` table `users`).




## Table `paramdef` {#core-ref:1ab061e2-2a87-4956-908f-16bb005d829b}

La table `paramdef` contient les définitions des [paramètres
applicatifs][paramapp].

| Colonne |         Type         |                                    Définition                                    |
| ------- | -------------------- | -------------------------------------------------------------------------------- |
| name    | text                 | Nom du paramètre                                                                 |
| isuser  | character varying(1) | Valeurs possibles : `Y` (paramètre utilisateur), `N` (paramètre non utilisateur) |
| isstyle | character varying(1) | *Obsolète* Non utilisé                                                           |
| isglob  | character varying(1) | Valeurs possibles : `Y` (paramètre global), `N` (paramètre applicatif)           |
| appid   | integer              | Identifiant de l'application                                                     |
| descr   | text                 | Description du paramètre                                                         |
| kind    | text                 | Type `text`, `enum`, `color`, `password`, `static` ou  `readonly`                |



![ Relations entre les tables définissant les paramètres ](advanced/dbparam.png)

<!-- links -->
[appact]:           #core-ref:b26f57fe-4d75-4d5f-a50e-129028b379ed "Applications et action"
[paramapp]:         #core-ref:c3d9cb18-16d0-435a-b8c2-5fa6ac06c522 "Les paramètres applicatifs"
[paramref]:         #core-ref:ec3701e6-fec8-45d9-afde-be61331d618b "Paramètres applicatifs de référence"
[parammgr]:         #core-ref:04dc3915-84db-4e2c-bb64-8dcc0ab26d0b "Parameter Manager"