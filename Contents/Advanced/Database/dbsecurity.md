# Descriptions des tables des droits documents et actions {#core-ref:60442310-845a-493b-a10d-09b5df0bb851}

## Table `docperm` {#core-ref:5cf15b7a-e8c8-4ec8-a3b8-2e676b6be349}

Les droits des [profils de documents][docprof] sont enregistrés dans la table
`docperm`.

Composition de la table `docperm` :

| Colonne |   Type  |                                          Définition                                          |
| ------- | ------- | -------------------------------------------------------------------------------------------- |
| docid   | integer | Identifiant du profil                                                                        |
| userid  | integer | Identifiant du compte (utilisateur, groupe ou rôle) ou attribut de document (table `vgroup`) |
| upacl   | integer | Droits du compte pour le profil                                                              |


La colonne `upacl` contient un masque binaire (int4 : 32bits) indiquant les
droits.


 |  31 | ... |  11 |      10      |   9    |   8    |    7    |        6         |            5            |  4   |   3    |  2   |  1   |  0  |
 | --- | --- | --- | ------------ | ------ | ------ | ------- | ---------------- | ----------------------- | ---- | ------ | ---- | ---- | --- |
 |     |     |     | confidential | unlock | modacl | viewacl | modify / icreate | open / create / execute | send | delete | edit | view |     |


Les droits 11 à 31 peuvent être utilisés pour indiquer des droits spécifiques.

Extrait de la table docperm :

    [sql]
    # SELECT docid, userid, upacl::bit(32) from docperm;
     docid | userid  |              upacl               
    -------+---------+----------------------------------
      4947 |       2 | 00000000000000000000000000100010
     15743 |       4 | 00000000000000000000000110000100
     15749 |       2 | 00000000000000000000000000100010
     15749 |       4 | 00000000000000000000000110000100
     15773 |     178 | 11111111111111111111111111111110
      2100 |     178 | 11111111111111111111111111111110
     15750 |       2 | 00000000000000000000000000100010


Dans cet extrait, la première ligne montre que le compte 2 (groupe all), pour le
profil 4967 (document rapport) a le droit de voir et d'exécuter la recherche.


Cette table ne permet pas de savoir directement si un utilisateur a tel ou tel
droit sur un document car il faut aussi corréler le résultat au groupe
d'appartenance et aux rôles affectés à l'utilisateur.

Pour connaître l'ensemble des documents que l'utilisateur 'john.doe' peut voir,
il est possible d'utiliser la colonne `views` de la table
[`family.documents`][dbdoc] ou [`docread`][docread].

    [sql]
    SELECT docread.id, docread.title, users.memberof  
    FROM docread, users 
    WHERE docread.views && ( users.memberof || '{0}'::int[] ) 
      AND users.login = 'john.doe';

Note : L'ajout du zéro dans la liste permet à la condition de récupérer aussi
les documents sans profil. Les documents sans profil ont `{0}` dans la colonne
`views`.

Pour connaître l'ensemble des documents que l'utilisateur 'john.doe' peut
modifier, il faut s'appuyer sur le contrôle du droit `edit` (position 2, masque 0x04) et
utiliser la fonction `hasaprivilege(userMemberOf, Profid, AccessBinaryMask)`.

    [sql]
    SELECT docread.id, docread.title, profid  
    FROM docread, users 
    WHERE hasaprivilege(users.memberof, docread.profid, 2<<1) 
      AND users.login = 'john.doe' ;



## Table `docpermext` {#core-ref:fbf760b5-98df-4261-83b6-9a1500082788}

La table `docpermext` indique les droits étendus. Ces droits sont utilisés
lorsque la liste des droits est variable.

Les [cycles de vies][wprof] et les [contrôles de vues][cvprof] utilisent les
droits étendus.

Le nom des droits pour les cycles est le nom de la transition. Le nom des droits
pour les contrôles de vue est l'identifiant de la vue.

Composition de la table `docpermext` :

| Colonne |   Type  |                                          Définition                                          |
| ------- | ------- | -------------------------------------------------------------------------------------------- |
| docid   | integer | Identifiant du profil (document cycle ou contrôle de vue)                                    |
| userid  | integer | Identifiant du compte (utilisateur, groupe ou rôle) ou attribut de document (table `vgroup`) |
| acl     | text    | Nom du droit                                                                                 |

## Table `permission` {#core-ref:d16b4f5f-34e2-4044-9776-b0ace68c1719}

La table `permission` indique les [droits applicatifs][acls] posés. Ce sont ces
droits qui sont utilisés pour vérifier l'accès à l'[exécution d'une
action][exeaction].

Composition de la table `permission` :

|    Colonne     |   Type  |                              Définition                             |
| -------------- | ------- | ------------------------------------------------------------------- |
| id_user        | integer | Identifiant du compte (utilisateur, groupe ou rôle) (table `users`) |
| id_application | integer | Identifiant de l'application (table `application`)                  |
| id_acl         | integer | Identifiant de l'acl applicative (table `acl`)                      |
| computed       | boolean | Cache valeur calculée - vrai si c'est un droit déduit               |

Des droits calculés (`computed=true`) sont enregistrés pour accélérer le
calcul du droit lors des prochaines requêtes.

## Table `acl` {#core-ref:c7caa985-3b34-4abd-8ffa-2e7110718efc}

La table `acl` contient la définition des [droits utilisées][acls] par les
applications.

|    Colonne     |     Type     |                                           Définition                                           |
| -------------- | ------------ | ---------------------------------------------------------------------------------------------- |
| id             | integer      | Identifiant de l'acl - issus de la séquence `seq_id_acl`                                       |
| id_application | integer      | Identifiant de l'application (table `application`)                                             |
| name           | text         | Nom de l'acl                                                                                   |
| description    | text         | Texte descriptif du droit                                                                      |
| group_default  | character(1) | Indique si cette acl est posée par défaut sur le groupe `all`. Valeurs possibles : [Y&#124;N]. |


![ Relations entre les tables mises en œuvre pour la vérification des droits d'accès ](advanced/dbprofils.png)

<!-- links -->
[docprof]:          #core-ref:f1575705-10e8-4bf2-83b3-4c0b5bfb77cf
[cvprof]:           #core-ref:65603797-5d8a-4a0d-954a-2dc69b5af11e "Profil de contrôle de vue"
[wprof]:            #core-ref:e0d99925-df0d-4d51-8ebc-d44c4dd03873 "Profil de cycle de vie"
[docread]:          #core-ref:378a7ed5-2703-4167-a980-c84bc29f8c56 "Table docread"
[dbdoc]:            #core-ref:d4b8d8ce-6f7a-4c1c-a5c4-f1adfcb74864 "Table doc"
[acls]:             #core-ref:a98b72ea-c063-4907-abc4-e5171ab55e59 "Les droits applicatifs"
[exeaction]:        #core-ref:14b2cf06-da86-46be-832e-942e842c9552 "Exécuter une action"