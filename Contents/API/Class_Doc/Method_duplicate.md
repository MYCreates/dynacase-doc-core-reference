# Doc::duplicate() {#core-ref:f7d4f454-0e45-40bd-9f4c-b149ab620295}

<div class="short-description" markdown="1">
Méthode utilisée pour dupliquer un document.
</div>

## Description {#core-ref:e54bb8a5-30d5-4cf4-8394-90823f9ea6d9}

    [php]
    Doc|string duplicate (bool $temporary = false, 
                          bool $control   = true, 
                          bool $linkfld   = false, 
                          bool $copyfile  = false )

Cette méthode réalise une copie de l'objet Document. Cette copie est enregistrée
en base de donnée avec un nouvel identificateur.

La révision de la copie est affectée à zéro. Le profil de la copie est affectée
au profil ([`cprofid`][famprop]) indiquée par la famille.

Si le document est lié à un cycle, l'état de la copie est l'état initial du
cycle.

### Avertissements {#core-ref:9df4da8c-342b-4893-9f34-3890d4b66807}

Aucun.

## Liste des paramètres {#core-ref:47883b87-b1d1-42bb-a21d-8077775e328d}


(bool) `temporary`
:   Si `true`, le document en base est créé comme document temporaire.  
    Aucun profil n'est associé au document s'il est temporaire.  
    Si `false` : le document est crée avec un identificateur non temporaire

(bool) `control`
:   Si `true`, la duplication est contrôlé par le droit `create`.  
    Si `false` : aucun contrôle de droit est réalisé.

(bool) `linkfld`
:   Si `true`, et si le document est un dossier. alors les documents contenus
    dans le dossier original sont liés aussi au dossier copié. Les documents
    contenus ne sont pas dupliqués mais juste liés.  
    Si `false` : seul le document dossier est copié.
:   
(bool) `copyfile`
:   Si `true`, les fichiers attachés au document (attribut de type 
    [`file`][attrfile] ou [`image`][attrimage]) sont dupliqués aussi.
    Si `false` : les fichiers sont liés aux fichiers originaux.


## Valeur de retour {#core-ref:50fcf35d-5731-4ce2-b617-f4f8b0313f36}

En cas d'erreur : Message d'erreur. Si la chaîne retournée est non nulle, le
message indique l'erreur qui a interdit la suppression.

En cas de succès : retourne l'objet `Doc` dupliqué.

## Erreurs / Exceptions {#core-ref:8f55f5fd-472b-4614-98a6-316ea9214d9c}

Retourne un message d'erreur dans les cas suivants :

*   pas de droit de créer un document de la même famille,
*   retour du [`preDuplicate()`][precopy] non vide,
*   des attributs sont en [visibilité][attrvis] `I`.

Exceptions :

-   `\Dcp\Exception('DOC0203')` si le document à dupliquer est invalide.

## Historique {#core-ref:814cfd8e-3541-4062-af7d-56f8add3411d}

Anciennement `Doc::copy()`.

## Exemples {#core-ref:90687694-4238-4662-a7c2-888463d0e217}

Duplication du document référencé par l'identificateur `$documentId`.

    [php]
    $doc=new_doc('', $documentId);
    if ($doc->isAlive()) {
        $copyDoc=$doc->duplicate();
        // maintenant le document est dupliqué
        if (is_string($copyDoc)) {
            $err=copyDoc;
            printf ("Erreur de duplication %s", $err );
        } else {
            printf("Duplication de %s [%d] => %s [%d]", 
                   $doc->getTitle(), $doc->id, 
                   $copyDoc->getTitle(), $copyDoc->id);
        }
    }

## Notes {#core-ref:bd7b0688-1023-4bf9-9c2b-6753073683c7}

Une entrée dans l'historique du document dupliqué indique l'origine de la
duplication.

Lors de la duplication à l'aide du menu "Dupliquer" (action
GENERIC:GENERIC_DUPLICATE), le document dupliqué est inséré dans le même
dossier primaire (propriété `prelid`) que l'origine.

## Voir aussi {#core-ref:7231f782-18b8-405e-8758-755c2d8c2a17}

Les hameçons (hooks) :

*   [`preDuplicate()`][precopy]
*   [`postDuplicate()`][postcopy]

<!-- links -->
[doccopy]:      #core-ref:f7d4f454-0e45-40bd-9f4c-b149ab620295
[precopy]:      #core-ref:c8d89bd7-e44c-4d30-b727-36f815a921db
[postcopy]:     #core-ref:d3a5ed05-5efd-441f-a276-c9423a89d874
[famprop]:      #core-ref:ccad55c7-5ff1-40c3-ad6f-ec79b1a9ac3c
[attrfile]:     #core-ref:0e904376-317c-426e-bc6d-e56fd52bad89
[attrimage]:    #core-ref:4fca7712-59e0-4186-bfd0-6214104a0f60
[attrvis]:      #core-ref:3e67d45e-1fed-446d-82b5-ba941addc7e8