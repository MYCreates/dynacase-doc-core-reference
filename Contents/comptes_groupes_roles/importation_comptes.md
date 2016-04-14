# Importations de comptes (document) {#core-ref:a0cb9a84-6bde-476c-b55c-95c8f12abd3a}

<span class="flag from release inline">3.2.21</span> Ce chapitre décrit une alternative
à l'[importation XML][importxml] en utilisant l'importation de document. Il est
toutefois préférable d'utiliser l'importation XML qui permet de mieux contrôler
les caractéristiques spécifique à l'importation de comptes. Ce mode
d'importation reste néanmoins opérationnel et est adapté pour la mise à jour de
données fonctionnelles enregistré sur les documents liés aux comptes.


L'importation de groupes, d'utilisateurs et de rôles est effectuée à l'aide d'un
fichier d'importation `CSV` (texte séparé par des point-virgules) ou `ODS` comme
pour les [importations de documents][import_documents] en général.

## Importation d'utilisateurs {#core-ref:c2b428cf-5271-4835-947c-3eba235cc579}

La création de compte utilisateur nécessite les 6 informations suivantes :

*   le nom `us_lname`
*   le prénom `us_fname`
*   le login `us_login`
*   le mot de passe `us_passwd1` et `us_passwd2`. Les deux mots de passe doivent être identiques.
*   le mail `us_extmail` facultatif, mais nécessaire pour récupérer son mot de passe oublié.

| #       | IUSER   | Identifiant | Groupe | login             | prénom     | nom        | mot de passe | confirmation | adresse mail       |
| -       |         |             |        |                   |            |            |              |              |                    |
| *ORDER* | *IUSER* |             |        | *us_login*        | *us_fname* | *us_lname* | *us_passwd1* | *us_passwd2* | *us_extmail*       |
| DOC     | IUSER   |             |        | robert.martin     | Robert     | Martin     | secret       | secret       | roby@somewhere.net |
| DOC     | IUSER   |             |        | isabelle.dujardin | Isabelle   | Dujardin   | secret       | secret       | isa@somewhere.net  |

## Importation de groupes {#core-ref:fa33d094-17ab-4309-82d4-0efece948cc0}

La création de groupes nécessite les 2 informations suivantes :

*   le nom `grp_name`
*   la référence `us_login`

| #       | IGROUP   | Identifiant | Groupe | nom                   | référence  |
| -       |          |             |        |                       |            |
| *ORDER* | *IGROUP* |             |        | *grp_name*            | *us_login* |
| DOC     | IGROUP   |             |        | Laboratoire recherche | labord     |
| DOC     | IGROUP   |             |        | Comité d'entreprise   | ce         |

## Importations de rôles {#core-ref:6747acce-c3c1-4970-bca2-c6fcd28930ff}

La création de groupes nécessite les 2 informations suivantes :

*   le nom `grp_name`
*   la référence `us_login`

| #       | ROLE   | Identifiant | Groupe | nom         | référence    |
| -       |        |             |        |             |              |
| *ORDER* | *ROLE* |             |        | *role_name* | *role_login* |
| DOC     | ROLE   |             |        | Concepteurs | designers    |
| DOC     | ROLE   |             |        | Conducteurs | drivers      |

**Note** : Le rôle ne peut pas être inséré dans un groupe.

## Affectation de groupes par importation {#core-ref:8da365c3-79a6-45b4-ae9e-93e59504323d}

L'affectation de groupes se fait en indiquant l'identifiant du groupe dans la
colonne *Identifiant* du fichier.

Dans l'exemple suivant :

*   l'utilisateur *Robert Martin* sera inséré dans le groupe
    *   *Laboratoire de recherche*
*   l'utilisatrice *Isabelle Dujardin* sera insérée dans les groupes
    *   *Laboratoire de recherche*
    *   *Comité d'entreprise*.

| #       | IGROUP   | Identifiant       | Groupe            | nom                      | référence  |
| -       |          |                   |                   |                          |            |
| *ORDER* | *IGROUP* |                   |                   | *grp_name*               | *us_login* |
| DOC     | IGROUP   | GRP_LABORECHERCHE |                   | Laboratoire de recherche | labord     |
| DOC     | IGROUP   | GRP_CE            |                   | Comité d'entreprise      | ce         |

| ##      | IUSER   | Identifiant | Groupe            | login             | prénom     | nom        | mot de passe | confirmation | adresse mail       |
| -       |         |             |                   |                   |            |            |              |              |                    |
| *ORDER* | *IUSER* |             |                   | *us_login*        | *us_fname* | *us_lname* | *us_passwd1* | *us_passwd2* | *us_extmail*       |
| DOC     | IUSER   |             | GRP_LABORECHERCHE | robert.martin     | Robert     | Martin     | secret       | secret       | roby@somewhere.net |
| DOC     | IUSER   | US_ISA      | GRP_CE            | isabelle.dujardin | Isabelle   | Dujardin   | secret       | secret       | isa@somewhere.net  |
| DOC     | IUSER   | US_ISA      | GRP_LABORECHERCHE |                   |            |            |              |              |                    |

## Affectation de rôles par importation {#core-ref:31758678-9322-45a3-b11c-3dfb13207209}

L'affectation de rôle à un utilisateur est fait avec l'attribut `us_roles`.

L'association de rôle à un groupe est fait avec l'attribut `grp_roles`. Ces deux
attributs sont multivalués et ils peuvent contenir plusieurs références séparé
par les deux caractères `\n`.

Dans l'exemple suivant, "Robert Martin" aura les rôles "Concepteurs" et
"Conducteur" car il appartient au groupe "Laboratoire annexe" qui est un sous-
groupe de "Laboratoire de recherche". "Isabelle Dujardin" aura les rôles
"Concepteur" et "Conducteur" car elle appartient au groupe "Laboratoire de
recherche" et "Chimiste" car ce rôle est explicitement indiqué dans l'attribut
`us_roles`.

| #       | ROLE   | Identifiant  | Groupe | nom         | référence    |
| -       |        |              |        |             |              |
| *ORDER* | *ROLE* |              |        | *role_name* | *role_login* |
| DOC     | ROLE   | ROLE_DESIGN  |        | Concepteurs | designers    |
| DOC     | ROLE   | ROLE_DRIVE   |        | Conducteurs | drivers      |
| DOC     | ROLE   | ROLE_CHEMIST |        | Chimiste    | chemist      |

| #       | IGROUP   | Identifiant       | Groupe            | nom                      | référence  | roles                   |
| -       |          |                   |                   |                          |            |                         |
| *ORDER* | *IGROUP* |                   |                   | *grp_name*               | *us_login* | *grp_roles*             |
| DOC     | IGROUP   | GRP_LABORECHERCHE |                   | Laboratoire de recherche | labord     | ROLE_DESIGN\nROLE_DRIVE |
| DOC     | IGROUP   | GRP_CE            |                   | Comité d'entreprise      | ce         | ROLE_DESIGN             |
| DOC     | IGROUP   | GRP_LABOANX       | GRP_LABORECHERCHE | Recherches annexes       | anx        |                         |

| #       | IUSER   | Identifiant | Groupe            | login             | prénom     | nom        | mot de passe | confirmation | adresse mail       | rôles        |
| -       |         |             |                   |                   |            |            |              |              |                    |              |
| *ORDER* | *IUSER* |             |                   | *us_login*        | *us_fname* | *us_lname* | *us_passwd1* | *us_passwd2* | *us_extmail*       | *us_roles*   |
| DOC     | IUSER   |             | GRP_LABOANX       | robert.martin     | Robert     | Martin     | secret       | secret       | roby@somewhere.net |              |
| DOC     | IUSER   | US_ISA      | GRP_CE            | isabelle.dujardin | Isabelle   | Dujardin   | secret       | secret       | isa@somewhere.net  | ROLE_CHEMIST |
| DOC     | IUSER   | US_ISA      | GRP_LABORECHERCHE |                   |            |            |              |              |                    |              |

<!-- links -->
[import_documents]: #core-ref:2fb3284a-2424-44b2-93ae-41dc3969e093
[importxml]:        #core-ref:d9e6f16a-6627-4f12-9d5f-a136b21e7cc3