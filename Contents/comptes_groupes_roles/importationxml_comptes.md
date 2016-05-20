# Importation des comptes en XML {#core-ref:d9e6f16a-6627-4f12-9d5f-a136b21e7cc3}

<span class="flag inline release from">3.2.21</span> Les comptes utilisateurs
peuvent être créés ou modifiés en important un fichier XML de description de
comptes. Ce fichier permet d'indiquer les données systèmes des comptes comme le
login, le mot de passe, les [groupes][group] d'appartenance et les [rôles][role]
associés.

L'importation purement système utilise le [schéma][xsd] qui ne référence que les
données système et aucune donnée documentaire fonctionnelle.

L'importation en plus des données systèmes peut aussi importer des [données
fonctionnelles][importaccountdoc] si des familles personnalisées ont été
définies pour être associées aux comptes.

## Importation de compte (XML) depuis l'interface d'administration {#core-ref:936975e6-b30f-4621-98cf-d01f41ad7630}

Depuis le centre d'administration, dans le gestion des utilisateurs, il est
possible d'importer les comptes (utilisateurs, groupes, rôles) depuis un fichier
XML.

![ Options d'importation de comptes ](importAccountsXML.png)

## Importation de compte (XML) en ligne de commande {#core-ref:f310eb79-27aa-4756-bfa6-c25c3a3758f4}

L'importation peut être réalisée en ligne de commande :

    ./wsh.php --api=importAccounts --file=/tmp/myUsers.xml --report=/tmp/myReport.json

Son usage est le suivant : 

    Usage :
        --file=<the input XML file>
       Options:
        --report-file=<the output report file>
        --dry-run (Analyse file only - no import is proceed) 

Le rapport est une sortie texte. Si le nom du fichier à l'extension ".csv" le
rapport est au format CSV. Si l'extension est ".json", le format du rapport est
en json.

Les informations retournées dans le rapport sont : 

* login : identifiant du compté traité
* action : code du traitement effectué
* error : message d'erreur (vide si pas d'erreur)
* message : message du traitement effectuée
* node : nœud XML qui est à l'initiative du traitement au traitement (non retourné pour le type texte)


Si une erreur est détectée aucun compte n'est mis à jour.

## Format d'importation XML d'utilisateurs {#core-ref:e9719360-c679-449a-ba6a-cb410b4b1d30}

### Importation minimaliste d'utilisateurs {#core-ref:b57e6cb3-c1fd-4d32-b773-48dc68c40ed8}

Les informations obligatoires pour importer un utilisateur sont :

*    `login` : identifiant du compte servant à l'authentification
*    `lastname` : nom à afficher pour le compte

<span class="flag inline nota-bene"/>  : le `login` est toujours en caractère
minuscule. L'authentification est "case insensitive" pour le login.

Le _login_ sert d'identifiant unique. Si un compte existe avec le _login_ défini,
ses informations seront mises à jour. S'il n'existe pas, le compte sera créé.

Exemple minimaliste d'importation de 2 comptes "un" et "deux": 

    [xml]
    <?xml version="1.0" encoding="utf-8"?>
    <accounts>
      <users>
        <user>
          <login>un</login>
          <lastname>Premier</lastname>
        </user>
        <user>
          <login>deux</login>
          <lastname>Deuxième</lastname>
        </user>
      </users>
    </accounts>

Ce fichier importe 2 comptes utilisateur. Ces comptes n'ont aucun [rôle][role]
et n'appartiennnent à aucun [groupe][group]. Ils n'ont pas de mot de passe et ne
peuvent se connecter. Par contre, ils sont opérationnels pour être utilisés dans
les [profils][profil].

### Autres informations optionnelles sur le compte {#core-ref:133e6a13-514e-4096-ae60-fbcac344561c}

Les informations systèmes optionnelles sont :

*   `mail` :    adresse courriel pour le compte. Cette adresse est notamment 
   utilisée pour la procédure de changement de mot de passe et pour les envois 
   de document par courriel.

*   `firstname`:    prénom de l'utilisateur. Le nom et le prénom constituent 
                le nom à afficher (displayName).

*   `status`: (booléen) Permet d'activer ou désactiver un compte. Par défaut, le
    compte est actif. Mettre `false` pour le désactiver. Un compte désactivé ne
    peut plus se connecter.

*   `substitute`: (référence) Permet de référencer un [suppléant][substitute]. 
    La référence est le **login** du compte suppléant. Le suppléant doit être défini
    avant le titulaire.

Exemple d'importation avec les données optionnelles :

Ce fichier vient compléter les informations du fichier ci-dessus en ajoutant  un
suppléant pour "un" et les adresses courriels pour les 2 comptes "un" et "deux".

    [xml]
    <?xml version="1.0" encoding="utf-8"?>
    <accounts>
      <users>
        <user>
          <login>deux</login>
          <lastname>Deuxième</lastname>
          <firstname>Gérard</firstname>
          <mail>second@example.net</mail>
          <status activated="true"/>
        </user>
        <user>
          <login>un</login>
          <lastname>Premier</lastname>
          <firstname>Isabelle</firstname>
          <mail>first@example.net</mail>
          <status activated="true"/>
          <substitute reference="deux"/><!-- la référence est défini avant -->
        </user>
      </users>
    </accounts>


### Mettre un mot de passe {#core-ref:ad82b7f3-46e0-4f88-a368-b15fa736e408}

Il est possible de définir un mot
de passe lors de la création ou de la modification du compte utilisateur.

La balise `password` peut contenir le mot de passe en clair ou crypté.
S'il est en clair, il sera crypté avant enregistrement, sinon il sera enregistré
directement. Le mode de [cryptage][phpcrypt] à utiliser est le [`SHA256`][sha256].

Mot de passe crypté :

    [xml]
    <password crypted="true">$5$6gyojNc42P8GMDB4$QbHmquZLRNwye3TjHaiHuoR6JnYmfwp8eWMD/hGLp93</password>

Mot de passe en clair :

    [xml]
    <password crypted="false">Mon secret</password>

### Affecter l'utilisateur à des groupes {#core-ref:ac9e4d26-7bab-49b2-ac27-d59e8bc6fe2c}

Les groupes d'appartenance directe sont définis dans la balise `parentGroups`.
Cette balise indique les références aux groupes parents (balises `parentGroup`).

L'attribut `reset` est un booléen qui indique si les groupes parents déjà
enregistrés doivent être enlevés avant d'ajouter les nouvelles références. Cet
attribut ne sert que dans le cas de mise à jour de compte. Si `reset` vaut
`false`  (valeur par défaut), les références aux groupes sont ajoutés aux
anciens groupes sinon les anciens groupes sont retirés du compte utilisateur.
L'attribut `reference` indique la référence du compte groupe et non le nom
logique du document représentant le groupe.

Exemple : ajout des groupes "vigilance" et "security"

    [xml]
    <parentGroups reset="false">
        <parentGroup reference="vigilance"/>
        <parentGroup reference="security"/>
    </parentGroups>



### Donner des rôles à un utilisateur {#core-ref:57bb1d66-ec37-4251-ac78-187294bdb026}

Les rôles de l'utilisateur sont définis dans la balise `associatedRoles`.
Cette balise indique les références aux différents rôles donnés (balises `associatedRole`).

L'attribut `reset` est un booléen qui indique si les rôle déjà
enregistrés doivent être enlevés avant d'ajouter les nouvelles références. Cet
attribut ne sert que dans le cas de mise à jour de compte. Si `reset` vaut
`false`  (valeur par défaut), les références aux rôles sont ajoutés aux
anciens rôles sinon les anciens rôles sont retirés du compte utilisateur.
L'attribut `reference` indique la référence du rôle et non le nom
logique du document représentant le rôle.

Exemple : ajout des rôles "veterinary" et "surgeon"

    [xml]
    <associatedRoles reset="false">
        <associatedRole reference="veterinary"/>
        <associatedRole reference="surgeon"/>
    </associatedRoles>


### Exemple récapitulatif {#core-ref:46537878-e61b-4f26-8fd0-50d53167ec75}

Importation du compte "garde" qui est dans le groupe "security" et qui
a le rôle "surveillant".

    [xml]
    <?xml version="1.0" encoding="utf-8"?>
    <accounts date="2016-04-14T09:54:18">
      <roles>
        <role>
          <reference>surveillant</reference>
          <displayName>Gardien surveillant</displayName>
        </role>
      </roles>
      <groups>
        <group >
          <reference>security</reference>
          <displayName>Surveillants</displayName>
          <parentGroups reset="false">
            <parentGroup reference="all"/>
          </parentGroups>
        </group>
      </groups>
      <users>
        <user>
          <login>garde</login>
          <firstname>Robert</firstname>
          <lastname>Dogue</lastname>
          <mail>garde@example.net</mail>
          <status activated="true"/>
          <password crypted="true">$5$PsPOxUFpskK25TY4$LjEnQqJw76duTmA9G7dd/XC9zexKgNanxz.3virIIRD</password>
          <associatedRoles reset="false">
            <associatedRole reference="surveillant"/>
          </associatedRoles>
          <parentGroups reset="false">
            <parentGroup reference="security"/>
          </parentGroups>
        </user>
      </users>
    </accounts>

## Importation XML de groupes {#core-ref:c1618ed2-e910-4f60-984c-a0b741708987}
### Importation minimaliste de groupe {#core-ref:e395c71e-cd75-474c-867b-5c2f8b9c5599}

Les informations obligatoires pour importer un [groupe][group] sont :

*   `reference` : référence unique du compte groupe
*   `displayName` : nom du groupe à afficher

<span class="flag inline nota-bene"/>  : la `reference` est toujours en caractère
minuscule. 

Exemple : création du groupe "business".

    [xml]
    <?xml version="1.0" encoding="utf-8"?>
    <accounts>
      <groups>
        <group>
          <reference>business</reference>
          <displayName>Service commercial</displayName>
        </group>
      </groups>
    </accounts>

### Définir une arborescence de groupe {#core-ref:f702b4c9-1181-442d-bc1a-2d9b971aa6c2}

Les balises d'affectation de groupe est identique aux [celles des
utilisateurs][parentGroup].

Exemple : 

<kbd>business</kbd> <kbd>sponsor</kbd>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#x021D8;<kbd>angels</kbd>&#x021D9;

    [xml]
    <?xml version="1.0" encoding="utf-8"?>
    <accounts >
      <groups>
        <group>
          <reference>business</reference>
          <displayName>Service commercial</displayName>
        </group>
        <group>
          <reference>sponsor</reference>
          <displayName>Mécènes</displayName>
        </group>
        <group >
          <reference>angels</reference>
          <displayName>Business angels</displayName>
          <parentGroups reset="false">
            <parentGroup reference="business"/>
            <parentGroup reference="sponsor"/>
          </parentGroups>
        </group>
      </groups>
    </accounts>


### Donner des rôles à un groupe  {#core-ref:d665f008-914a-4939-9f8b-baa81603b5c1}

Les rôles sont associés à un groupe de la [même manière][assocrole] que pour les
utilisateurs.

## Importation XML de rôles {#core-ref:ec461788-4379-4f48-b090-9de0df0e66ec}


Les informations obligatoires pour importer un [rôle][role] sont :

*   `reference` : référence unique du compte rôle
*   `displayName` : nom du rôle à afficher

<span class="flag inline nota-bene"/>  : la `reference` est toujours en caractère
minuscule. 

    [xml]
    <?xml version="1.0" encoding="utf-8"?>
    <accounts>
      <roles>
        <role>
          <reference>watcher</reference>
          <displayName>Surveillant</displayName>
        </role>
        <role>
          <reference>veterinary</reference>
          <displayName>Vétérinaire</displayName>
        </role>
      </roles>
    </accounts>

## Importation d'utilisateurs avec données fonctionnelles personnalisées {#core-ref:46255ff7-5aae-46fc-b96f-25d2788a7660}

Les comptes sont représentés par des documents. Ces documents permettent
d'interagir avec les comptes systèmes.

Les familles utilisées par défaut pour chaque type de compte sont :

*   Utilisateur: Famille [IUSER][IUSER]
*   Groupe: Famille [IGROUP][IGROUP]
*   Rôle: Famille [ROLE][ROLE]

La balise `document` permet de modifier le document lié au compte.
Elle contient la représentation [XML du document][docxml] lié.

### Poser un nom logique sur le document associé au compte {#core-ref:72a12727-cd07-4b51-88f6-1108dc9677c5}

Le nom logique est indiqué sur l'attribut `name` de la balise du document lié.

Exemple : Mettre le nom logique "USER_CONTROL" sur le compte "control".

    [xml]
    <?xml version="1.0" encoding="utf-8"?>
    <accounts date="2016-04-14T11:40:22">
      <users>
        <user id="1">
          <login>control</login>
          <firstname>Robert</firstname>
          <lastname>Watson</lastname>
          <document family="IUSER">
            <iuser name="USER_CONTROL" />
          </document>
        </user>
      </users>
    </accounts>

### Modifier le date d'expiration du compte {#core-ref:2e7e6977-856f-48f6-a171-571262080dfd}

La date d'expiration n'est pas une donnée du compte système. Elle est définie
dans la famille `IUSER` dans l'attribut [`us_accexpiredate`][us_accexpiredate].

    [xml]
    <?xml version="1.0" encoding="utf-8"?>
    <accounts>
      <users>
        <user>
          <login>watch.garde</login>
          <firstname>Robert</firstname>
          <lastname>Dogue</lastname>
          <mail>garde@example.net</mail>
          <document family="IUSER">
            <iuser name="MY_BIGGARDE" >
              <us_tab_system>
                <us_fr_security>
                  <us_accexpiredate>2016-04-13</us_accexpiredate>
                </us_fr_security>
              </us_tab_system>
            </iuser>
          </document>
        </user>
      </users>
    </accounts>

### Associer un compte avec une famille personnalisée {#core-ref:64bb9f7b-e55e-4ffe-b078-ea95417cfeb1}

Pour utiliser une famille personnalisée, il est nécessaire que celle-ci soit
dérivée des familles par défaut (`IUSER`, `IGROUP` ou `ROLE`).

Le format XML du document associé doit suivre le  [schéma de la
famille][docxmlschema] donné. L'ensemble des schémas XML nécessaires peut être
obtenu depuis le centre d'administration. Il est accessible en cliquant sur le
bouton "Importer les comptes" depuis l'[interface de gestion des utilisateurs][ihmimport].

Exemple : associer le compte avec un document de la famille "my_watcher". Cette
famille héritant de la famille `IUSER`, permet d'indiquer les heures de gardes
et la zone à garder.

    [xml]
    <?xml version="1.0" encoding="utf-8"?>
    <accounts>
      <users>
        <user>
          <login>watch.garde</login>
          <firstname>Robert</firstname>
          <lastname>Watson</lastname>
          <mail>watch.garde@example.net</mail>
          <associatedRoles reset="false">
            <associatedRole reference="surveillant"/>
          </associatedRoles>
          <parentGroups reset="false">
            <parentGroup reference="all"/>
            <parentGroup reference="security"/>
          </parentGroups>
          <document family="MY_WATCHER">
            <my_watcher name="MY_BIGWATCH" >
              <watch_tab_watching>
                <watch_fr_watching>
                  <watch_beginhour>10:00:00</watch_beginhour>
                  <watch_endhour>18:00:00</watch_endhour>
                  <watch_array_zone>
                    <watch_zone name="MY_ZONE51">Zone protégée</watch_zone>
                  </watch_array_zone>
                </watch_fr_watching>
              </watch_tab_watching>
            </my_watcher>
          </document>
        </user>
      </users>
    </accounts>

<!-- links -->
[xsd]:  https://github.com/Anakeen/dynacase-xml-schemas/blob/master/accounts-1.0.xsd "Schéma système d'importation de compte"
[profil]:      #core-ref:f1575705-10e8-4bf2-83b3-4c0b5bfb77cf
[group]:    #core-ref:e01fc125-52ef-4c48-b4c6-95ddeac23327
[role]:     #core-ref:6017a086-3211-485f-b68a-b93850953065
[substitute]:   #core-ref:1591eb1c-aead-4f7b-bde9-5f42e397b22e
[parentGroup]:  #core-ref:ac9e4d26-7bab-49b2-ac27-d59e8bc6fe2c
[phpcrypt]: http://php.net/manual/fr/function.crypt.php "PHP crypt"
[sha256]:   https://fr.wikipedia.org/wiki/SHA-2#SHA-256
[assocrole]:    #core-ref:57bb1d66-ec37-4251-ac78-187294bdb026
[IUSER]:    #core-ref:4842d6c2-f88d-41d9-aa30-04437ffdb67e
[IGROUP]:   #core-ref:6afad021-29c2-400b-87f2-3a83551e3e95
[ROLE]:     #core-ref:f0abd65e-c453-443e-ab39-bc5ae6ee6f1f
[docxml]:   #core-ref:4d098747-0120-4ead-b13e-97ef1250e08d
[us_accexpiredate]: #core-ref:3c68911e-bee2-40b2-9f28-6c0ec68e4077
[docxmlschema]:  #core-ref:d28f9655-2db3-4073-a8bf-cb1edd89ed8d
[importaccountdoc]: #core-ref:46255ff7-5aae-46fc-b96f-25d2788a7660
[ihmimport]:    #core-ref:936975e6-b30f-4621-98cf-d01f41ad7630