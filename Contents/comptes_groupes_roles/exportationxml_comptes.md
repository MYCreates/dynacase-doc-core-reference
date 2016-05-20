# Exportation XML des comptes {#core-ref:3c9fc09e-4f7c-466e-a207-c0fdf948fe97}

<span class="flag inline release from">3.2.21</span> Les formats d'exportation 
des comptes suivent les mêmes schémas que ceux de
l'[importation XML][importXML].

Les fichiers produits par l'exportation peuvent être réutilisés pour l'importation.
Ils ne contiennent pas de données calculées exploitables par l'importation.

Exemple d'exportation : 1 utilisateur et 2 groupes

    [xml]
    <?xml version="1.0" encoding="utf-8"?>
    <accounts date="2016-04-14T14:42:15">
      <groups>
        <group id="2">
          <reference>all</reference>
          <displayName>Utilisateurs</displayName>
          <document family="IGROUP"/>
        </group>
        <group id="10">
          <reference>security</reference>
          <displayName>Surveillants</displayName>
          <parentGroups reset="false">
            <parentGroup reference="all"/>
          </parentGroups>
          <document family="IGROUP"/>
        </group>
      </groups>
      <users>
        <user id="15">
          <login>watcher</login>
          <firstname>Robert</firstname>
          <lastname>Dogue</lastname>
          <mail>watcher@example.net</mail>
          <status activated="true"/>
          <password crypted="true">$5$PsPOxUFpfkK25TY4$LjEnQaJw76duTzM9G7dq/XC9zexKgNOnxz.3virIIRD</password>
          <associatedRoles reset="false">
            <associatedRole reference="surveillant"/>
          </associatedRoles>
          <parentGroups reset="false">
            <parentGroup reference="security"/>
          </parentGroups>
          <document family="IUSER"/>
        </user>
      </users>
    </accounts>

L'exportation fournie la date d'exportation sur la balise racine (attribut
`date`). Sur chaque compte, l'identifiant numérique interne est indiqué
(attribut `id`). Cet attribut n'est pas utilisé pour l'importation. Il est
donné à titre indicatif.

## Exportation par l'interface d'administration {#core-ref:6e85ec08-c513-45e5-abab-8b3e700b6c88}

L'exportation des comptes peut être réalisée depuis le centre d'administration
dans la gestion des utilisateurs.

![ Options d'exportation de comptes ](exportAccountsXML.png)

*   Exporter avec les schémas XML (zip format)
:   Inclus un répertoire XSD dans lequel tous les schémas XML nécessaires à la 
    validation de fichiers d'importation sont présents

*   Exporter les mots de passe cryptés
:   Inclus la balise `password` avec le mot de passe crypté pour les utilisateurs
    (le mot de passe en clair n'est pas enregistré)

*   Exporter les rôles associés
:   Inclus la définitions des rôles associés pour les utilisateurs et les groupes

*   Exporter les groupes des utilisateurs
:   Inclus la définition des groupes d'appartenances (groupes directs et indirects)

*   Exporter les données documentaires spécifiques
:   Inclus la définition du document associé au compte. Les [données redondantes][IUSER] 
    avec les données systèmes inclus dans le document ne sont pas exportés.

L'exportation via l'interface, exporte les comptes affichés au moment de
l'exportation.

## Exportation de comptes en ligne de commande {#core-ref:b48718fb-c02c-475b-acf3-76c73708952f}

L'exportation en ligne de commande est réalisé avec le script "exportAccounts"

    ./wsh.php --api=exportAccounts --help
    
    Export accounts definition
    Usage :
        --file=<the output file (use - for stdout)>
       Options:
        --schema-directory=<directory where produce xsd for documents>
        --memberOf=<Restrict to account which are member of this group or role reference>
        --type=<restricted to account type> [user|role|group] 
        --login-filter=<filter login contains>
        --help (Show usage) 
        --crypt-password (add crypt password) 
        --roles (export associated roles) 
        --groups (export parent groups) 
        --document (export specific document information) 

Sans aucun argument autre que le fichier, cette commande exporte l'ensemble des 
comptes dans le fichier XML défini en sortie.


<!-- links -->
[IUSER]:    #core-ref:4842d6c2-f88d-41d9-aa30-04437ffdb67e
[importXML]:#core-ref:d9e6f16a-6627-4f12-9d5f-a136b21e7cc3