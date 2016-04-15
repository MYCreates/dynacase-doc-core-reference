# Historique de la documentation {#core-ref:e4cf4232-38e7-4673-afd1-5730c1a95c48}

Ce chapitre contient un descriptif des améliorations entre les releases de 
Dynacase.


## Édition 10 {#core-ref:94453692-21ca-4200-83cc-597a71400801}

|                      Modifications                       |                                        Chapitre                                        |                   Version                   |
| :------------------------------------------------------- | :------------------------------------------------------------------------------------- | :------------------------------------------ |
| Explications de l'objet de partage de document           | [Objet de partage de document][shareddoc]                                              | <span class="flag new">Nouveau</span>       |
| Précision sur les exportations vis-à-vis des révisions   | [Révision et exportation][exportrevision], [Révision XML][revisionxml]                 | <span class="flag update">Mis à jour</span> |
| Ajout information sur la révision pour formatCollection  | [Formatage des relations][fmtDocId]                                                    | <span class="flag update">Mis à jour</span> |
| Paramétrage du pied de document                          | [Document footer][docfoot]                                                             | <span class="flag new">Nouveau</span>       |
| Ajout de parties variables dans le paramètre MAIL_ACTION | [Paramètre MAIL_ACTION][mailaction]                                                    | <span class="flag update">Mis à jour</span> |
| Facilité d'importation et d'exportation de comptes       | [Importation XML de comptes][ixmlaccounts], [Exportation XML de comptes][exmlaccounts] | <span class="flag new">Nouveau</span>       |
| Ajout de la propriété "exists" pour les énumérés         | [Formatage des énumérés][fmtenum]                                                      | <span class="flag update">Mis à jour</span> |
| Explication classe `UpdateAttribute`                     | [Mise à jour par lot][updateAttribute]                                                 | <span class="flag new">Nouveau</span>       |
| Modification sur l'identififiant de fichier              | [Table vaultdiskstorage][vaultid]                                                      | <span class="flag update">Mis à jour</span> |

## Édition 9  {#core-ref:568e128a-3a2b-493c-b59d-1c5cce8ae515}

|                                   Modifications                                    |                                   Chapitre                                  |                   Version                   |
| :--------------------------------------------------------------------------------- | :-------------------------------------------------------------------------- | :------------------------------------------ |
| Nouveau hook Doc::preAffect / postAffect                                           | [Hook d'affectation][prepostaffect]                                         | <span class="flag new">Nouveau</span>       |
| Nouvelle option d'attribut htmltext et vérification de la validité                 | [allowedcontent][htmltextopt]                                               | <span class="flag new">Nouveau</span>       |
| Ajout description des méthodes addWarningMsg et addLogMsg                          | [Action::addwarningmsg()][addwarningmsg],  [Action::addLogmsg()][addlogmsg] | <span class="flag update">Mis à jour</span> |
| Précision sur la détection paramètre des CSV lors de l'importation                 | [Précaution sur l'importation de document][importlimits]                    | <span class="flag update">Mis à jour</span> |
| Orientation des images jpeg                                                        | [Vue des attributs images][imgview]                                         | <span class="flag update">Mis à jour</span> |
| Script `wstop` et `wstart`                                                         | [`wstop`][wstop], [`wstart`][wstart]                                        | <span class="flag update">Mis à jour</span> |
| Ajout option `--cmd=unregister-all` à [manageContextCrontab][manageContextCrontab] | [manageContextCrontab][manageContextCrontab]                                | <span class="flag update">Mis à jour</span> |

## Édition 8 {#core-ref:f10874a8-78c8-42eb-9908-5eaaf04d247f}

|                                                                      Modifications                                                                       |                                  Chapitre                                  |                   Version                   |
| :------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------- | :------------------------------------------ |
| Règle globale pour les styles                                                                                                                            | [Règles de style][globalstyle]                                             | <span class="flag new">Nouveau</span>       |
| Ajout explication pour les paramètres CORE_TMPDIR, CORE_TMPDIRMAXAGE, CORE_LOGDURATION, CORE_SESSIONMAXAGE, CORE_SESSIONGCPROBABILITY, FREEDOM_UPLOADDIR | [Paramètre système][paramSystem]                                           | <span class="flag new">Nouveau</span>       |
| Description de la table doclog                                                                                                                           | [table doclog][doclog]                                                     | <span class="flag new">Nouveau</span>       |
| Fichier openDocument Writer : Précisions sur les images insérées dans les attributs "htmltext"                                                           | [Image et htmltext][convooo]                                               | <span class="flag update">Mis à jour</span> |
| Précision sur les critères relations dans les recherches détaillées                                                                                      | [Recherche détaillée][relsearch]                                           | <span class="flag update">Mis à jour</span> |
| Possibilité de déclarer des attributs obligatoires dans les tableaux                                                                                     | [Attribut obligatoire][requires]                                           | <span class="flag new">Nouveau</span>       |
| Nouvelle option pour les nombres dans l'exportation CSV pour les rapports                                                                                | [Rapport][exportnumber]                                                    | <span class="flag new">Nouveau</span>       |
| Prise en compte de la préaffectation lors de la création de document pour les vues spécifiques                                                           | [Action OPENDOC][opendocnew]                                               | <span class="flag new">Nouveau</span>       |
| Ajout lien vers les codes d'erreur de l'API PHP pour l'importation de documents                                                                          | [Importer des documents par ligne de commande][importationswsh_error_code] | <span class="flag update">Mis à jour</span> |
| Si une ligne `ORDER` est erronée, l'import de documents de cette famille est ignoré                                                                      | [Ordre des attributs][attributes_order]                                    | <span class="flag update">Mis à jour</span> |
| Précisions sur les options de l'attribut Htmltext                                                                                                        | [Options Htmltext][htmlopt]                                                | <span class="flag update">Mis à jour</span> |
| Comportement de la contrainte lorsque l'attribut passé est de type array                                                                                 | [Syntaxe des contraintes][constraints_syntax]                              | <span class="flag update">Mis à jour</span> |
| Utilisation du contexte dans les traductions javascript                                                                                                  | [Traduction js][i18nsjs]                                                   | <span class="flag update">Mis à jour</span> |
| Possibilité de sélectionner un "Document destinataire" ccomme destinataire pour un modèle de mail                                                        | [Spécification de l'émetteur ou du destinataire][document_destinataire]    | <span class="flag new">Nouveau</span>       |
| Ajout options `--status-file` et `--stop-on-error` au script d'API `refreshDocuments`, et changement du comportement en cas d'erreur                     | [refreshDocuments][api_refreshDocuments]                                   | <span class="flag update">Mis à jour</span> |
| Ajout "Fonction de rappel à l'extinction"                                                                                                                | [Fonction de rappel à l'extinction][shutdown_function]                     | <span class="flag new">Nouveau</span>       |

## Édition 7 {#core-ref:67c3551d-d99a-43c9-b837-5d93439bff8e}

|                                  Modifications                                   |                                                          Chapitre                                                          |                    Version                     |
| :------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------- |
| Ajout callstack dans le retour d'erreur de wsh                                   | [Erreur wsh][wsherror]                                                                                                     | <span class="flag update">Mise à jour</span>   |
| Précisions sur les fonctions de transaction                                      | [Transactions][savepoint]                                                                                                  | <span class="flag update">Mise à jour</span>   |
| Précisions sur les minuteurs avec date dynamique                                 | [Minuteur dynamique][timerdyn]                                                                                             | <span class="flag update">Mise à jour</span>   |
| Précisions sur les mécanismes d'exception                                        | [Exception][Exception]                                                                                                     | <span class="flag new">Nouveau chapitre</span> |
| Ajout verrouillage de transaction                                                | [Transaction][Transaction]                                                                                                 | <span class="flag update">Mise à jour</span>   |
| Niveau de log paramétrable par instance                                          | [Log::setLogLevel][setloglevel]                                                                                            | <span class="flag new">Nouveauté</span>        |
| Nouveau test d'enregistrement des entiers                                        | [Type int][typeint]                                                                                                        | <span class="flag new">Nouveauté</span>        |
| Nouveaux hook dans formatCollection                                              | [Classe FormatCollection][formatcollection]                                                                                | <span class="flag new">Nouveauté</span>        |
| Modification du rendu (formatCollection) des longtext présents dans les tableaux | [FormatCollection - longtext][fmtlongtext]                                                                                 | <span class="flag update">Mise à jour</span>   |
| Impacts de la visibility I des attributs                                         | [Importation wsh][importwsh], [interface][importcaution], [exportation][exportcaution], [format de collection][fmtcollout] | <span class="flag update">Mise à jour</span>   |
| Précision sur l'itération de document avec SearchDoc                             | [SearchDoc::setObjectReturn][setobjectreturn]                                                                              | <span class="flag update">Mise à jour</span>   |
| L'authentification par jetons n'émet plus de cookies de session                  | [Authentification par jetons][authtype_open]                                                                               | <span class="flag update">Mise à jour</span>   |


## Édition 6 {#core-ref:24bca82b-3ce1-49df-b866-2442d8cfec64}

|                                Modifications                                |                     Chapitre                     |                    Version                     |
| :-------------------------------------------------------------------------- | :----------------------------------------------- | :--------------------------------------------- |
| Ajout des nouvelles options d'export CSV                                    | [CSV exportations][csv_export]                   | <span class="flag update">Mise à jour</span>   |
| Ajout de l'option displayrowcount sur les array                             | [Array displayrowcount][display_row_count]       | <span class="flag update">Mise à jour</span>   |
| Ajout de l'option SET sur l'importation des profils                         | [importation des profil][import_profil]          | <span class="flag update">Mise à jour</span>   |
| Précision sur l'option showempty pour les images                            | [showempty][commonoptions]                       | <span class="flag update">Mise à jour</span>   |
| Précision sur la composition d'un titre                                     | [Titre de document][famattrtitle]                | <span class="flag update">Mise à jour</span>   |
| Précision sur le retour d'un paramètre de famille                           | [Doc::getFamilyParameter()][returnfamilyparam]   | <span class="flag update">Mise à jour</span>   |
| Modification du libellé du droit `modify` de dossier                        | [Droit `modify`][profildoc]                      | <span class="flag update">Mise à jour</span>   |
| Précision sur l'inclusion de css                                            | [Application::addCssRef()][addcssref]            | <span class="flag update">Mise à jour</span>   |
| Modification du retour SearchDoc::onlyCount() en cas d'erreur               | [SearchDoc::onlyCount()][searchdocOnlycount]     | <span class="flag update">Mise à jour</span>   |
| Avertissement sur l'utilisation de getCustomTitle dans les rapports         | [Doc::getCustomTitle()][customtitlew]            | <span class="flag update">Mise à jour</span>   |
| Modification visuel des attributs de type `file` et `image` en modification | Attributs [file][attrfile] et [image][attrimage] | <span class="flag update">Mise à jour</span>   |
| Option d'attribut `elabel` indiqué comme options communes d'attribut        | [Option elabel][elabel]                          | <span class="flag update">Mise à jour</span>   |
| Gestion des minuteurs                                                       | [Méthode pour les minuteurs][doctimer]           | <span class="flag new">Nouveau chapitre</span> |


## Édition 5 {#core-ref:2c9c3f1e-d1b6-4d13-84b2-a3e3cbf0fa53}

|              Modifications              |              Chapitre              |                   Version                    |
| :-------------------------------------- | :--------------------------------- | :------------------------------------------- |
| Précision pour les rapports             | [Doc::getCustomTitle][customtitle] | <span class="flag update">Mise à jour</span> |
| Précision paramètre OPENDOC             | [FDL:OPENDOC][opendoc]             | <span class="flag update">Mise à jour</span> |
| Précision paramétrage du modèle de mail | [Modèle de mail][mailtpl]          | <span class="flag update">Mise à jour</span> |


## Édition 4 {#core-ref:ee35db85-a173-4840-b6d9-ce26eb93e01b}

L'édition 4 de la documentation a modifié les points suivants.


|                  Modifications                  |                     Chapitre                    |                    Version                     |
| :---------------------------------------------- | :---------------------------------------------- | :--------------------------------------------- |
| Description de la mise en place des traductions | [Internationalisation][i18n]                    | <span class="flag new">Nouveau chapitre</span> |
| Ajout chapitres sur les templates               | [Usage avancée des templates][advtemplate]      | <span class="flag new">Nouveau chapitre</span> |
| Ajout graphe d'accès                            | [Cinématique de dynacase][cinematique]          | <span class="flag new">Nouveau chapitre</span> |
| Description des principales tables              | [La base de données][database]                  | <span class="flag new">Nouveau chapitre</span> |
| Famille processus                               | [Famille processus][processus]                  | <span class="flag new">Nouveau chapitre</span> |
| Ajout chapitres Dbobj, QueryDb, Transaction     | [Mécanismes de persistance][persist]            | <span class="flag new">Nouveau chapitre</span> |
| Ajout chapitre compte                           | [Manipulation des comptes utilisateur][account] | <span class="flag new">Nouveau chapitre</span> |
| Ajout chapitre migration                        | [Migration des applications][migration]         | <span class="flag new">Nouveau chapitre</span> |
| Ajout chapitre contrôle d'accès                 | [Contrôle des accès][accesscontrol]             | <span class="flag new">Nouveau chapitre</span> |
| Ajout chapitre zones et actions de référence    | [Zone et actions de référence][zoneref]         | <span class="flag new">Nouveau chapitre</span> |
| Ajout chapitre SearchDoc                        | [Classe SearchDoc][searchdoc]                   | <span class="flag new">Nouveau chapitre</span> |
| Mise à jour des chapitres API                   | [Les essentiels de l'API][apichapter]           | <span class="flag new">Mise à jour</span>      |
| Ajout chapitre Utilitaire gestion de documents  | [Utilitaire gestion de documents][utilDoc]      | <span class="flag new">Nouveau chapitre</span> |


## Modification release 3.2.12 {#core-ref:d402539b-f0dd-4ade-9ea0-03f1d55da1da}

### Internationalisation {#core-ref:ccb46f7b-04f3-4398-b3f4-a09bf9eb508c}

Ajout de la possibilité d'utiliser les [contextes][i18nctx] et les [formes
plurielles][i18nplural] dans les traductions.

### SearchDoc::addGeneralFilter {#core-ref:95ea5aea-add4-4807-9a5c-c04ac1e87966}

La méthode [`SearchDoc::addGeneralFilter()`][searchdocAddGeneralFilter] retourne
systématiquement une exception en cas d'erreur.

### SearchDoc::join {#core-ref:80bd51a7-7db2-45ad-9556-21dc3b56b311}

La méthode [`SearchDoc::join()`][searchdocJoin] retourne une
exception en cas d'erreur.

### SearchDoc::onlyCount {#core-ref:f99061ff-3ec1-4a6f-95b3-5841b6fec880}

La méthode [`SearchDoc::onlyCount()`][searchdocOnlycount] effectue
systématiquement un appel à la base de donnée pour récupérer le résultat.

### SearchDoc::setRecursiveSearch {#core-ref:de878fe6-b2d5-47f8-9bd5-94d8eb2aeeff}

La méthode [`SearchDoc::setRecursiveSearch()`][searchdocrecursivesearch] a un
nouveau paramètre pour indiquer le niveau de profondeur. Ceci évite de mettre à
jour directement la propriété `SearchDoc::folderRecursiveLevel`.

### Importation CSV {#core-ref:a5fa61d8-3a4e-4c9c-9867-8dfae1bdfb29}

Le script [`importDocument`][wshimportDocuments] a de nouvelles options
permettant de configurer l'importation des formats [`csv`][CSV].

L'interface d'administration d'importation des documents permet aussi de
configurer les options d'importation pour les fichiers `csv`.

### Layout::eSet, Layout::eSetBlockData {#core-ref:4d01a2c8-0f6b-4c29-a64c-4b8fbef5b127}

Les méthodes [`Layout::eSet()`][layouteset] et 
[`Layout::eSetBlockData()`][layoutesetblock] ont été ajoutées afin de faciliter
l'ajout de clefs correctement encodées dans des fichiers XML et HTML.

### Dir::insertMultipleDocuments {#core-ref:b66ef951-c5ee-4ee0-9499-7913ed805042}

La méthode [`Dir::insertMultipleDocuments`][insertMultipleDocuments] a été
modifiée afin de faire remonter le message d'erreur de la méthode hameçon
[`Dir::postInsertMultipleDocuments`][postinsertMultipleDocuments] dans son
retour d'erreur.

<!-- link -->
[insertMultipleDocuments]:      #core-ref:098cf44e-568d-4dd2-8dd0-e2f104bc8615
[postinsertMultipleDocuments]:  #core-ref:e3cd509f-8678-4dec-a0cf-33aa39674cfe
[layoutesetblock]:      #core-ref:088e711c-ea91-45e7-841d-289ffc53c80b
[layouteset]:           #core-ref:2696710a-f491-4887-b953-e08d918ef4fb
[wshimportDocuments]:   #core-ref:a14d9475-0431-4aa3-853d-810b61e355a7
[histo]:                #core-ref:e4cf4232-38e7-4673-afd1-5730c1a95c48
[persist]:              #core-ref:5f09399c-bb49-4033-90d6-c04876948269
[account]:              #core-ref:68c93fb2-088c-435a-b4ac-e1b94095d0c9
[cinematique]:          #core-ref:24705f94-2dee-4e84-9429-d89dafe83589
[advtemplate]:          #core-ref:af9ea76c-069e-49e1-a382-efc8ca35f1eb
[database]:             #core-ref:e97a35de-f7f4-465d-8b2d-5c7bab5656eb
[i18n]:                 #core-ref:8f3ad20a-4630-4e86-937b-da3fa26ba423
[processus]:            #core-ref:4a65995d-a61d-4325-89e2-1a9ce15f76e8
[migration]:            #core-ref:d2bd57f9-7b5a-46b0-8570-6b5b0710d7c3
[accesscontrol]:        #core-ref:8d73fa24-b721-4a16-a34b-846004e3e9ca
[zoneref]:              #core-ref:fed06a0c-3fd6-11e3-9658-88d5dc830245
[searchdoc]:            #core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255
[searchdocAddGeneralFilter]:    #core-ref:453cff11-09d9-4607-ab81-7acd36e99750
[searchdocJoin]:                #core-ref:c7fe0a1b-e71a-45d4-9182-9e4561558030
[searchdocOnlycount]:           #core-ref:2d43be1a-1991-42dd-a25d-5c3bb0b393fa
[searchdocrecursivesearch]:     #core-ref:b99a6125-5a8b-420b-b1ce-f6a459f11612
[CSV]: http://fr.wikipedia.org/wiki/Comma-separated_values "Comma-separated values sur wikipedia"
[i18nplural]:           #core-ref:3e6b8eee-4171-11e3-9688-cffb8e583c34
[i18nctx]:              #core-ref:3275febc-4171-11e3-9773-cffb8e583c34
[apichapter]:           #core-ref:0c6d26ba-ab12-4659-aaf9-bcad5a1194ef
[Dir::insertMultipleDocuments]: #core-ref:098cf44e-568d-4dd2-8dd0-e2f104bc8615
[Dir::postInsertMultipleDocuments]: #core-ref:e3cd509f-8678-4dec-a0cf-33aa39674cfe
[utilDoc]:                          #core-ref:deb7de49-dbfb-4feb-8f35-cc9aedf352a2
[customtitle]:              #core-ref:3c5ff78d-c080-48fb-a293-9736ed4e95b8
[opendoc]:          #core-ref:f9e68fa7-01b7-4903-9718-744271d63112
[mailtpl]:          #core-ref:8723b1aa-10d3-4316-af6b-071f4d59ceee
[commonoptions]:    #core-ref:16e19c90-3233-11e2-a58f-6b135c3a2496
[famattrtitle]:      #core-ref:b0e414c0-b795-4bbe-b70e-a308b7f1b4ab
[returnfamilyparam]: #core-ref:7cffbb46-353a-4072-9bca-1773599857dc
[profildoc]:         #core-ref:f1575705-10e8-4bf2-83b3-4c0b5bfb77cf
[addcssref]:         #core-ref:4bba8a6b-8002-4c0a-8ac7-70d75b31b02b
[customtitlew]:      #core-ref:d7c909a8-f2fa-4ddf-954c-00704e9a694d
[attrfile]:         #core-ref:0e904376-317c-426e-bc6d-e56fd52bad89
[attrimage]:        #core-ref:4fca7712-59e0-4186-bfd0-6214104a0f60
[elabel]:           ./core-ref:16e19c90-3233-11e2-a58f-6b135c3a2496.html#commonELabel "Option commune de elabel"
[doctimer]:          #core-ref:6403d0d7-9e4c-42e9-8a07-a2256a7c43f7
[import_profil]:    #core-ref:2ec1ae6f-4b2a-4bc2-a100-4e5873538bb5
[display_row_count]: #core-ref:9eb6f53f-158d-497d-a472-2602a195cbce
[csv_export]: #core-ref:83ec5f8c-c048-4da9-ab81-edd5d52efc0d
[wsherror]:         #core-ref:982b9e0c-56ef-40c4-a8f8-0ae0826f07a2
[savepoint]:        #core-ref:32e0a8cb-0e8b-4f77-a62d-a45da16d39a8
[timerdyn]:         #core-ref:386637d4-ab5b-4b3b-bf80-f2e6c226c555
[Exception]:        #core-ref:4bc5157a-5dbc-4d87-b1e5-ece7e104dc20
[Transaction]:      #core-ref:14fd71fa-1944-4016-80ab-6616e3423ce7
[setloglevel]:      #core-ref:c654a501-5cfa-4951-a5b0-8e7be4741fa0
[typeint]:          #core-ref:eda3132e-f8a6-421b-ae07-d36615b705ec
[formatcollection]: #core-ref:d25bfead-3edf-4d35-abff-f4892b0237f2
[fmtlongtext] :     #core-ref:0dca5896-2598-4f7c-bb81-1cf4a82c7024
[importwsh]:        #core-ref:1c97f553-dcba-454e-96a0-8059230065b3
[setobjectreturn]:  #core-ref:867fcba6-94e7-403e-a523-73e20583a25f
[authtype_open]:    #core-ref:9edc8f2e-6929-11e2-8610-0021e9fffec1
[importcaution]:    #core-ref:a3f0e390-b967-4de4-8bf8-85bedb173085
[fmtcollout]:       #core-ref:da2ae3f0-af0c-4747-92e6-0999f8f05ffe
[exportcaution]:    #core-ref:88fb91b5-51a3-4b33-ac2e-5f20eddd8210
[globalstyle]:      #core-ref:cc5b14c2-c671-49dd-879f-398bdeb3c9da
[paramSystem]:      #core-ref:a4c9dd9a-201c-40af-8f31-3a70d7b2765b
[doclog]:           #core-ref:9090c9ee-fb9c-4cca-b9d6-962052ed69a9
[convooo]:          #core-ref:6ad0e889-a8d8-4245-9051-a7ada2e31c86
[relsearch]:        #core-ref:1ced6e00-055c-4959-836d-00ed077d14c8
[requires]:         #core-ref:8c74cf5b-3e03-480f-ba05-1a86ea6ec634
[exportnumber]:     #core-ref:cf6934f9-afe2-48ea-877a-7a1cdc9f770e
[opendocnew]:       #core-ref:00345042-c8a2-44a9-a351-43e646b09b0b
[importationswsh_error_code]: #core-ref:1ab32c44-3233-4de6-bede-97f0aa58e617
[attributes_order]: #core-ref:e41116ee-a682-4033-a7ab-22dc1b99e56a
[constraints_syntax]: #core-ref:28bfa7d5-918e-4f47-a28b-44ceefcd0a23
[htmlopt] :         #core-ref:8e182116-8762-4157-a743-9abf43db0960
[i18nsjs]:          #core-ref:5bd6f306-4171-11e3-999b-60d7dc830245
[shutdown_function]: #core-ref:9f3475a2-b6a5-4927-b6b7-97f128cb4cd4
[document_destinataire]: #core-ref:e717367f-7a29-473d-a65a-ac2c924bd0cb
[api_refreshDocuments]: #core-ref:d42dccaf-2225-4727-b528-b66df42aa358
[prepostaffect]:        #core-ref:e11b3532-6d5b-4a1a-ad20-0667409f1f65
[htmltextopt]:          #core-ref:8e182116-8762-4157-a743-9abf43db0960
[addwarningmsg]:        #core-ref:4ee92978-bed2-4c2a-8e1a-04d37b1a3328
[addlogmsg]:            #core-ref:1e4c336f-f2af-462b-86d5-938f6b385b79
[importlimits]:         #core-ref:ab8856e9-1850-46d9-ae22-20fb54f9c078
[imgview]:              #core-ref:ce33d1ac-a7b6-4129-b8f2-ee5e11c02055
[wstop]:                #core-ref:f88c39a9-dc65-4443-a344-ab0ab56f4ca1
[wstart]:               #core-ref:04a8b73b-c1b3-44a9-bb19-6a712592a7f3
[manageContextCrontab]: #core-ref:6aa4de16-1e51-49df-9926-7079aab0d268
[shareddoc]:            #core-ref:947948f6-242c-40a7-8f70-8013fe2ab1f1
[exportrevision]:       #core-ref:c44857d2-6a33-4552-af61-3aafad359416
[revisionxml]:          #core-ref:7b0f2e8f-bdd0-44b1-b664-2ab4e3975740
[fmtDocId]:             #core-ref:95aee029-f729-41f9-9b18-a6c20813c24d
[docfoot]:              #core-ref:bc027994-70fd-4c17-9f55-577a21f717e6
[mailaction]:           #core-ref:c1d9e009-49a5-47a4-9104-4d044ea24aa3
[ixmlaccounts]:         #core-ref:d9e6f16a-6627-4f12-9d5f-a136b21e7cc3
[exmlaccounts]:         #core-ref:3c9fc09e-4f7c-466e-a207-c0fdf948fe97
[fmtenum]:              #core-ref:ddcd138f-acbd-46f0-bf82-2227399536dc
[updateAttribute]:      #core-ref:c28bea37-1f15-4157-aa79-40b5181d53f5
[vaultid]:              #core-ref:4e91a88e-66a3-46e7-824d-d11adb0c39fe