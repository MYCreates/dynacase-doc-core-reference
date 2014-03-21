# Description des tables des définition des applications {#core-ref:97b84670-2cfe-42c4-b237-3bbef354f65d}

Ce chapitre définit les tables qui contiennent les définitions des
[applications][appact] enregistrées. 

## La table `application` {#core-ref:3f46999b-3111-4ee6-8526-7a6e6f42be49}

La table `application` contient la partie _identification_ indiquée dans le fichier de
description de l'[application][app].

|   Colonne   |     Type     |                                  Définition                                 |
| ----------- | ------------ | --------------------------------------------------------------------------- |
| id          | integer      | Identificateur de l'application (issue de la séquence `seq_id_application`) |
| name        | text         | Nom de l'application                                                        |
| short_name  | text         | Description courte                                                          |
| description | text         | Description longue                                                          |
| access_free | character(1) | *Obsolète* Non utilisé  <span class="flag release obsolete">3.2.0</span>    |
| available   | character(1) | Valeurs possibles : `Y` (pour disponible) ou `N` (non disponible)           |
| icon        | text         | Nom du fichier icône de l'application                                       |
| displayable | character(1) | Valeurs possibles : `Y` (pour affichable) ou `N` (non affichable)           |
| with_frame  | character(1) | *Obsolète* Non utilisé   <span class="flag release obsolete">3.2.0</span>   |
| childof     | text         | Nom de l'application parente - Vide si pas de parent                        |
| objectclass | character(1) | *Obsolète* Non utilisé  <span class="flag release obsolete">3.2.0</span>    |
| ssl         | character(1) | *Obsolète* Non utilisé  <span class="flag release obsolete">3.2.0</span>    |
| machine     | text         | *Obsolète* Non utilisé   <span class="flag release obsolete">3.2.0</span>   |
| iorder      | integer      | *Obsolète* Non utilisé  <span class="flag release obsolete">3.2.0</span>    |
| tag         | text         | Balise applicative. Permet d'identifier des groupes d'applications          |

## La table `action` {#core-ref:07a1af43-e051-474c-a4c8-b32bf4245a6b}

La table `action` contient la partie [_définition des actions_][action] indiquée
dans le fichier de description de l'[application][app].




|    Colonne     |     Type     |                                                            Définition                                                           |
| -------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| id             | integer      | Identificateur de l'action (issue de la séquence `seq_id_action`)                                                               |
| id_application | integer      | Identificateur de l'application associée                                                                                        |
| name           | text         | Nom de l'action                                                                                                                 |
| short_name     | text         | Description courte                                                                                                              |
| long_name      | text         | Description longue                                                                                                              |
| script         | text         | Fichier PHP du contrôleur                                                                                                       |
| function       | text         | Fonction du contrôleur                                                                                                          |
| layout         | text         | Template de l'action                                                                                                            |
| available      | character(1) | Valeurs possibles : `Y` (pour disponible) ou `N` (non disponible)                                                               |
| acl            | text         | Nom de l'acl nécessaire pour exécuter l'action                                                                                  |
| grant_level    | integer      | *Obsolète* Non utilisé   <span class="flag release obsolete">3.2.0</span>                                                       |
| openaccess     | character(1) | Valeurs possibles : `Y` (pour utilisation possible en [authentification par jetons][openaccess]) ou `N` (dans le cas contraire) |
| root           | character(1) | Valeurs possibles : `Y` (action principale) ou `N` (action non principale)                                                      |
| icon           | text         | Icône de l'action                                                                                                               |
| toc            | character(1) | *Obsolète* Non utilisé   <span class="flag release obsolete">3.2.0</span>                                                       |
| father         | integer      | *Obsolète* Non utilisé   <span class="flag release obsolete">3.2.0</span>                                                       |
| toc_order      | integer      | *Obsolète* Non utilisé   <span class="flag release obsolete">3.2.0</span>                                                       |

## La table `acl` {#core-ref:cb5cf06a-d859-4c2f-b35f-3f70492612ed}

La table `acl` contient la partie [_définition des acl_][acls] indiquée
dans le fichier de description de l'[application][app].

Se reporter au chapitre  [Descriptions des tables des droits][dbacl].

![ Relations entre les tables définissant les applications ](advanced/dbapp.png)



<!-- links -->
[appact]:           #core-ref:b26f57fe-4d75-4d5f-a50e-129028b379ed "Applications et action"
[app]:              #core-ref:395f44f1-6699-4ad8-b525-31e65e9b6efb "Écrire une application"
[action]:           #core-ref:e67d8aeb-939c-46e3-9be8-6fc3ba75ebc2 "Écrire une action"
[openaccess]:       #core-ref:9edc8f2e-6929-11e2-8610-0021e9fffec1 "Authentification par jetons"
[dbacl]:            #core-ref:c7caa985-3b34-4abd-8ffa-2e7110718efc "Table acl"
[acls]:             #core-ref:a98b72ea-c063-4907-abc4-e5171ab55e59 "Les droits applicatifs"