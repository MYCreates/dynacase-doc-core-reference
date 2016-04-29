# Exportation des profils  {#core-ref:602c6331-7cdb-4b24-8a56-ffd11e00502f}

L'option d'exportation _"Avec les profils"_ permet d'enregistrer les profils
associés aux documents en plus des données du documents.

Cette option n'est applicable qu'avec le format _"Données réimportables"_.

Si le document a un profil lié alors le document _Profil_ sera aussi exporté.

Si le document a un profil dédié, la clef `PROFIL` portera sur le document même.

Si le profil lié est un profil dynamique, le profil calculé ne sera pas exporté,
mais le profil dynamique sera exporté.

Soit les 4 documents suivants :

*   Rotor :
    *   Profil lié : _ZOO_PRF_CLASSIFICATION_.
    *   Nom logique : _aliRotor_.
*   Théodor :
    *   Profil lié : _ZOO_PRF_CLASSIFICATION_.
    *   Pas de nom logique.
*   Éléonore :
    *   Profil lié : _ZOO_PRF_CLASSIFICATION_.
    *   Pas de nom logique.
*   Gastor :
    *   Profil dédié.
    *   Pas de nom logique.

Le fichier _csv_ produit sera comme décrit ci-dessous :

|   //FAM    |            animal(ZOO_ANIMAL)           |               Identifiant               |  Dossier  |          nom          |         espèce        |         classe        |                            |
| ---------- | --------------------------------------- | --------------------------------------- | --------- | --------------------- | --------------------- | --------------------- | -------------------------- |
| ORDER      | ZOO_ANIMAL                              |                                         |           | an_nom                | an_espece             | an_classe             |                            |
| DOC        | ZOO_ANIMAL                              | aliRotor                                |           | Rotor                 | ZOO_ESP_ALLI          | Reptilia              |                            |
|            |                                         |                                         |           |                       |                       |                       |                            |
| _//FAM_    | _profil de document(PDOC) _             | _Identifiant_                           | _Dossier_ | _titre_               | _description _        | _family id_           | _famille_                  |
| ORDER      | PDOC                                    |                                         |           | ba_title              | prf_desc              | dpdoc_famid           | dpdoc_fam                  |
| DOC        | PDOC                                    | __ZOO_PRF_CLASSIFICATION__              |           | Classification        |                       |                       |                            |
| __PROFIL__ | __ZOO_PRF_CLASSIFICATION__              | :useAccount                             |           | view=role_vetérinaire | edit=role_vetérinaire | view=role_surveillant | view=attribute(an_gardien) |
| __PROFIL__ | aliRotor                                | __ZOO_PRF_CLASSIFICATION__              |           |                       |                       |                       |                            |
| ORDER      | ZOO_ANIMAL                              |                                         |           | an_nom                | an_espece             | an_classe             |                            |
| DOC        | ZOO_ANIMAL                              | TEMPORARY_ZOO_ANIMAL_2079_51a707c53aa48 |           | Théodor               | ZOO_ESP_ALLI          | Reptilia              |                            |
| __PROFIL__ | TEMPORARY_ZOO_ANIMAL_2079_51a707c53aa48 | __ZOO_PRF_CLASSIFICATION__              |           |                       |                       |                       |                            |
| DOC        | ZOO_ANIMAL                              | TEMPORARY_ZOO_ANIMAL_2080_51a707c54ae36 |           | Éléonore              | ZOO_ESP_ALLI          | Reptilia              |                            |
| __PROFIL__ | TEMPORARY_ZOO_ANIMAL_2080_51a707c54ae36 | __ZOO_PRF_CLASSIFICATION__              |           |                       |                       |                       |                            |
| DOC        | ZOO_ANIMAL                              | TEMPORARY_ZOO_ANIMAL_3296_51a707c54ce55 |           | Gastor                | ZOO_ESP_ALLI          | Reptilia              |                            |
| __PROFIL__ | TEMPORARY_ZOO_ANIMAL_3296_51a707c54ce55 | :useAccount                             |           | view=role_surveillant | view=role_vetérinaire | edit=role_vetérinaire |                            |

Un nom logique temporaire est généré pour les documents n'ayant pas de nom
logique. Cet identifiant temporaire est supprimé tous les soirs avec le
programme _wsh_ [`cleanContext`][cleancontext].

La clef `PROFIL` contient l'ensemble des droits explicites mis sur les
[profils][profilage].

<span class="flag from release inline">3.2.21</span> L'affectation des droits
est faite avec les références (login) des [comptes][userid] _Utilisateurs_,
_Groupes_ et _Rôles_ .

Les différents notations de profils sont :

*   `[aclName]=[accountIdentifier]`
*   `[attributeName]=attribute([attributeIdentifier])` // Cas des profils dynamiques

**Attention** : Par défaut, l'importation des éléments ci-dessus ne fait
qu'ajouter les nouveaux droits et ne supprime pas les droits supprimés. Il
existe différentes [options][options_profil_import] pour l'importation des
profils permettant de  modifier ce comportement.

## Exportation de profil de famille {#core-ref:8701cd51-0767-4620-8770-57dff9c4460a}

Si le dossier à importer comporte un document _famille_ alors l'exportation du
profil exportera les documents suivants :

*   Le document profil de la famille 
*   La définition du profil de la famille
*   Le document cycle de vie par défaut de la famille et son profil
*   Les masques du cycles de vie et leur profil
*   Les modèles de mail du cycles de vie et leur profil

|            |                                              |                                           |         |                   |                                      |
| ---------- | -------------------------------------------- | ----------------------------------------- | ------- | ----------------- | ------------------------------------ |
| //FAM      | profil de document(PDOC)                     | Identifiant                               | Dossier | titre             | description                          |
| ORDER      | PDOC                                         |                                           |         | ba_title          | prf_desc                             |
| DOC        | PDOC                                         | ZOO_PRF_CLASSIFICATION                    |         | Classification    |                                      |
| __PROFIL__ | ZOO_PRF_CLASSIFICATION                       | :useAccount                               |         | view=gadmin       | viewacl=gadmin                       |
| //FAM      | modèle de mail(MAILTEMPLATE)                 | Identifiant                               | Dossier | Titre             | Famille                              |
| ORDER      | MAILTEMPLATE                                 |                                           |         | tmail_title       | tmail_family                         |
| DOC        | MAILTEMPLATE                                 | TEMPORARY_MAILTEMPLATE_3801_51a8c23b99679 |         | Couriel rédacteur | ZOO_DEMANDEADOPTION                  |
| //FAM      | profil de document(PDOC)                     | Identifiant                               | Dossier | titre             | description                          |
| ORDER      | PDOC                                         |                                           |         | ba_title          | prf_desc                             |
| DOC        | PDOC                                         | PRF_ADMIN_EDIT                            |         | Administration    | lecture seule sauf pour groupe admin |
| __PROFIL__ | PRF_ADMIN_EDIT                               | :useAccount                               |         | view=all          | edit=gadmin                          |
| __PROFIL__ | TEMPORARY_MAILTEMPLATE_3801_51a8c23b99679    | PRF_ADMIN_EDIT                            |         |                   |                                      |
| //FAM      | Cycle demande Adoption(ZOO_WDEMANDEADOPTION) | Identifiant                               | Dossier | titre             | description                          |
| ORDER      | ZOO_WDEMANDEADOPTION                         |                                           |         | ba_title          | wf_desc                              |
| DOC        | ZOO_WDEMANDEADOPTION                         | ZOO_CYCLEDA                               |         | Défaut            |                                      |
| //FAM      | profil de document(PDOC)                     | Identifiant                               | Dossier | titre             | description                          |
| ORDER      | PDOC                                         |                                           |         | ba_title          | prf_desc                             |
| DOC        | PDOC                                         | ZOO_PRF_HYGIENE                           |         | Hygiène           |                                      |
| __PROFIL__ | ZOO_PRF_HYGIENE                              | :useAccount                               |         | view=gadmin       | viewacl=gadmin                       |
| //FAM      | masque de saisie(MASK)                       | Identifiant                               | Dossier | titre             | Famille                              |
| ORDER      | MASK                                         |                                           |         | ba_title          | msk_famid                            |
| DOC        | MASK                                         | TEMPORARY_MASK_3802_51a8c23bac7c6         |         | Initialisé        | ZOO_DEMANDEADOPTION                  |
| __PROFIL__ | TEMPORARY_MASK_3802_51a8c23bac7c6            | PRF_ADMIN_EDIT                            |         |                   |                                      |
| //FAM      | profil de famille(PFAM)                      | Identifiant                               | Dossier | titre             | description                          |
| ORDER      | PFAM                                         |                                           |         | ba_title          | prf_desc                             |
| DOC        | PFAM                                         | ZOO_PRF_FAM                               |         | Profil Zoo        | Pour les familles du zoo             |
| __PROFIL__ | ZOO_PRF_FAM                                  | :useAccount                               |         | edit=gadmin       | viewacl=gadmin                       |
|            |                                              |                                           |         |                   |                                      |
| BEGIN      |                                              |                                           |         |                   | ZOO_DEMANDEADOPTION                  |
| __PROFID__ | ZOO_PRF_FAM                                  |                                           |         |                   |                                      |
| WID        | ZOO_CYCLEDA                                  |                                           |         |                   |                                      |
| END        |                                              |                                           |         |                   |                                      |

Les masques et les modèles de mail auront un nom logique temporaire s'ils n'ont
pas de nom logique.  
Le fichier exporté comporte aussi, à la fin, le paramétrage du profil et du
cycle pour la famille exportée.

**Note** : Les documents _"famille"_ (caractéristiques et structures) ne sont
 pas exportables. Seul leur profil est exportable avec cette option.

**Attention** : Par défaut, l'import des éléments ci-dessus ne fait qu'ajouter les nouveaux droits et ne supprime
pas les droits supprimés. Il existe différentes [options][options_profil_import] pour l'import des profils permettant de
 modifier ce comportement.

<!-- links -->
[profilage]: #core-ref:ce576351-dbe6-45d1-8097-f9573502b651
[options_profil_import]: #core-ref:2ec1ae6f-4b2a-4bc2-a100-4e5873538bb5
[cleancontext]:  #core-ref:100b123b-da1a-45b4-848b-0622f3e09a40
[userid]:        #core-ref:4842d6c2-f88d-41d9-aa30-04437ffdb67e