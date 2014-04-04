# Doc::hasPermission() {#core-ref:96f8ce9d-7bfb-4027-8106-10cbe8ee6a1a}

<div class="short-description" markdown="1">

Cette méthode permet de vérifier que l'utilisateur courant possède un
[droit][acldoc] particulier sur le document.

</div>


## Description {#core-ref:a8c62e72-3884-46e0-8a8e-d131633c7569}

    [php]
    bool hasPermission ( string $aclName, 
                           bool $strict = false )

Cette méthode retourne `true`, si l'utilisateur possède le droit indiqué. Ce
droit est indiqué dans le [profil][profil] du document et peut être obtenu par
un [rôle][role] ou un [groupe][group].

### Avertissements {#core-ref:e9bec10c-7611-4fb0-95fd-e36fdf24abdf}

Aucun.

## Liste des paramètres {#core-ref:9daaabab-7586-4a1e-ab7d-1f06120d3372}


(string) `aclName`
:   Nom du [droit][acldoc] a examiner.

(bool) `strict`
:   Indique s'il faut tenir compte de la [suppléance][suppleant] :
    
    -   Si `false` (par défaut) : les droits des titulaires sont aussi pris en
        compte
    -   Si `true` : les droits des titulaires ne sont pas pris en compte.

## Valeur de retour {#core-ref:1fbcbab4-b29d-4274-8a87-64cf2b683358}

Retourne `true` dans les cas suivants :

*   si l'utilisateur possède le droit demandé,
*   si l'utilisateur est "admin" (même si le droit n'existe pas).

Retourne `false` dans les cas suivants :

*   si l'utilisateur ne possède pas le droit demandé (hors "admin"),
*   si le droit n'est pas un des droits du document (hors "admin").


## Erreurs / Exceptions {#core-ref:f0df572b-2c8e-4000-9210-0b081834b878}

Aucune.

## Historique {#core-ref:14b415cf-c759-4f4c-90c8-12ee3a37b26a}

Cette méthode n'est disponible que depuis la version 3.2.5. 

La méthode `Doc::control()` disponible dans les versions précédentes permet de
réaliser le même test de droit. À la différence de `Doc::hasPermission()`, cette
dernière retourne un message d'erreur si le droit n'est pas obtenu sinon elle
retourne un message vide.

## Exemples {#core-ref:374e0a50-fb3b-4845-864a-f80ad6162fcb}

### Vérification du droit de modification

Le droit de modifier est le droit `edit`.

    [php]
    require_once("FDL/Class.Doc.php");
    $doc = \Dcp\DocManager::getDocument( "1420"); // document n°1420
    if ($doc && $doc->isAlive()) {
        if ($doc->hasPermission('edit')) {
            printf('Utilisateur "%s" a le droit de modifier "%s"'."\n",
                getCurrentUser()->login,
                $doc->getTitle()
            );
        } else {
            printf('Utilisateur "%s" n\'a pas le droit de modifier "%s"'."\n",
                getCurrentUser()->login,
                $doc->getTitle()
            );
        }
    } else {
        printf("Document non trouvé\n");
    }

Attention le droit `edit` ne suffit pas forcément pour autoriser la
modification. Il peut être nécessaire de vérifier aussi le verrou
([propriété][docprop] `locked`).

### Vérification de tous les droits

La liste des droits du documents est dans la propriété `acls` de l'objet.

    [php]
    require_once("FDL/Class.Doc.php");
    $doc = \Dcp\DocManager::getDocument( "1420");
    if ($doc && $doc->isAlive()) {
        $acls = $doc->acls;
        printf('Utilisateur "%s" a les droits suivants pour "%s" :'."\n",
            getCurrentUser()->login,
            $doc->getTitle()
        );
        foreach ($acls as $acl) {
            printf("Droit %-12s : %s\n", 
                $acl, 
                ($doc->hasPermission($acl))?"Oui":"Non"
            );
        }
    } else {
        printf("Document non trouvé\n");
    }

Résultat :

    Utilisateur "zoo.garde" a les droits suivants pour "Théodor" :
    Droit view         : Oui
    Droit edit         : Non
    Droit delete       : Non
    Droit send         : Non
    Droit unlock       : Non
    Droit confidential : Non
    Droit wask         : Non


## Notes {#core-ref:88fd47ce-13aa-4190-8c4f-76db350c16fa}

Les méthodes [`Doc::enableEditControl()`][enableedit] et
[`Doc::disableEditControl()`][disableedit] n'ont pas d'impact sur cette méthode.

## Voir aussi {#core-ref:621ca9e4-073d-40db-bbea-fad695c5ca49}

[La sécurité des documents][security].

<!-- links -->

[acldoc]:       #core-ref:a99dcc5f-f42f-4574-bbfa-d7bb0573c95d
[profil]:       #core-ref:f1575705-10e8-4bf2-83b3-4c0b5bfb77cf
[group]:        #core-ref:6afad021-29c2-400b-87f2-3a83551e3e95
[role]:         #core-ref:6017a086-3211-485f-b68a-b93850953065
[docprop]:      #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353
[suppleant]:    #core-ref:1591eb1c-aead-4f7b-bde9-5f42e397b22e
[security]:     #core-ref:bab5a377-c8cb-49ce-85c4-5162d05490a1
[enableedit]:   #core-ref:3c9aba8e-50a6-41b0-82e9-64f63085e5e5
[disableedit]:  #core-ref:b9844f4b-3e5c-4bbe-98ae-e71a4ba17239
