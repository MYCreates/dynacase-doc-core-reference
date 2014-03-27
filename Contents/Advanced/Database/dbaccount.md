# Description des tables des comptes utilisateurs {#core-ref:6d5684f4-73e8-431c-8b2b-6224a9e6b074}

Les tables `users` et `groups` sont les tables qui contiennent les données
systèmes des [comptes utilisateurs][users]. 

La tables `users` contient la définition des utilisateurs, groupes et rôles
utilisés par le système.

Composition de la table `users` :

|   Colonne   |     Type     |                                                 Modificateurs                                                 |
| ----------- | ------------ | ------------------------------------------------------------------------------------------------------------- |
| id          | integer      | Identifiant unique - Issue de la séquence `seq_id_users`                                                      |
| lastname    | text         | Nom                                                                                                           |
| firstname   | text         | Prénom                                                                                                        |
| login       | text         | Identifiant de connexion                                                                                      |
| password    | text         | Mot de passe crypté                                                                                           |
| substitute  | integer      | Identifiant du compte qui est suppléant de ce compte (restreint aux utilisateurs)                             |
| accounttype | character(1) | `U` : Utilisateur, `G` : Groupe, `R` : Rôle                                                                   |
| memberof    | integer[]    | Liste des identifiants des groupes et rôles du compte (calculé à partir de la table `groups`)                 |
| status      | character(1) | `A` : Actif, `D` : Désactivé                                                                                  |
| mail        | text         | Adresse principale de courriel                                                                                |
| fid         | integer      | Identifiant du document associé (pointe vers `doc128` (Utilisateurs), `doc130` (Rôles) ou `doc127` (Groupes)) |


Les identifiants inférieurs à 10 sont réservés au système.

| Identifiant |   Login   |        Définition          | Nom logique |
| ----------- | --------- | -------------------------- | ----------- |
|           1 | admin     | Identifiant administrateur | USER_ADMIN  |
|           2 | all       | groupe "Utilisateur"       | GDEFAULT    |
|           3 | anonymous | Identifiant accès anonyme  | USER_GUEST  |
|           4 | gadmin    | groupe "Administrateur"    | GADMIN      |

Les comptes sont liés à un document associé. Ce document constitue l'interface
entre les interfaces d'administration et le compte système. Lorsqu'un document
de compte (famille `IUSER`, `IGROUP` ou `ROLE`) est modifié le compte système
est alors modifié en conséquence.


L'appartenance d'un utilisateur à un groupe ou à un rôle, ou l'appartenance
d'un groupe à un autre groupe ou à un autre rôle, est fait par la table
`groups`.

Composition de la table `groups` :


| Colonne |   Type  |                                Définition                               |
| ------- | ------- | ----------------------------------------------------------------------- |
| iduser  | integer | identifiant d'utilisateur ou de groupe (table `users`)                  |
| idgroup | integer | identifiant de groupe ou de rôle appartenant à _iduser_ (table `users`) |
|         |         |                                                                         |


![ Lien entre les tables users et doc ](advanced/dbaccount.png)


<!-- links -->
[users]:        #core-ref:2bd98eec-5b03-4af0-b9d6-1bbf78fe9733 "Utilisateurs, groupes et rôles"