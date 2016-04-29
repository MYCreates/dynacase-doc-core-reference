# Doc::delete() {#core-ref:c4372b13-c132-4148-9487-de2b7614d497}

<div class="short-description" markdown="1">
Méthode utilisée pour supprimer un document.
</div>

## Description {#core-ref:00d0521d-cb59-444c-a423-49f03da968c7}

    [php]
    string delete ( bool $really  = false, 
                    bool $control = true, 
                    bool $nopost  = false )

Cette méthode met le statut du document à "supprimé". La [propriété][docprop]
`doctype` est affectée à `Z` (Zombie) et `locked` est affectée à `-1` dans le
cas d'une suppression logique.

### Avertissements {#core-ref:7f58a2d6-2123-4d52-b7d3-cfe8a2516ebd}

Cette méthode supprime toutes les [révisions][revision] du document.

## Liste des paramètres {#core-ref:d199b08d-6d29-4055-8451-a28eae527f3c}


(bool) `really`
:   Si `really` est `true`, le document ainsi que toutes ses révisions sont 
    supprimées de la base de données. Cette opération n'est pas annulable.  
    Si `really` est `false`, le document est supprimé de manière logique. il
    est donc toujours présent en base de données mais juste noté comme étant
    supprimé.

(bool) `control`
:   Si `control` est `true`, le droit `delete` est vérifié pour l'utilisateur
    courant. Si l'utilisateur n'a pas ce privilège la suppression est abandonnée.  
    Si `control` est `false`, la vérification des droits n'est pas effectuée.


(bool) `nopost`
:   Si `nopost` est `true`, alors les hameçons [`Doc::preDelete()`][docpreDelete]
    et [`Doc::postDelete()`][docpostDelete] ne sont pas exécutés.  
    Si `nopost` est `false`, ces deux hameçons (hooks) sont exécutés.

## Valeur de retour {#core-ref:ceab5069-5cf5-40da-a1a8-978959d45b8b}

Message d'erreur. Si la chaîne retournée est non nulle, le message indique
l'erreur qui a interdit la suppression.

## Erreurs / Exceptions {#core-ref:54be7a63-b1bb-4967-a879-41228a3ac404}

Une erreur est retournée si :

*   le document n'existe pas ou plus,
*   le document est déjà supprimé,
*   l'utilisateur n'a pas le droit `delete` sur le document,
*   l'hameçon [`preDelete()`][docpredelete] retourne une erreur.

## Historique {#core-ref:8f52ffbd-5957-4cf9-8354-5145ec21e8d8}

La méthode obsolète `Doc::reallyDelete()` est équivalente à cette méthode avec
le paramètre `$really` mis à `true`.

## Exemples {#core-ref:1b6ea473-0a7e-419b-b944-680689875695}

Suppression du document référencé par l'identificateur `$documentId`.

    [php]
    $doc=new_doc('', $documentId);
    if ($doc->isAlive()) {
        $err=$doc->delete();
        // maintenant le document est supprime
        if ($err == "") {
            printf("Suppression %s [%d]", $doc->getTitle(), $doc->id);
        }
    }

## Notes {#core-ref:13ec4b3e-ceae-49a0-a581-35cf9fc9585e}

Une entrée dans l'historique est enregistrée à chaque suppression.

Les documents supprimés logiquement sont toujours accessibles avec leur
identifiant. Ils ne peuvent par contre plus être modifiés par l'interface web.

La classe [`SearchDoc`][searchdoc] permet de rechercher les documents supprimés
en utilisant l'attribut `trash`.

## Voir aussi {#core-ref:219225dc-7c14-44ed-95fa-3c94b14dd49f}

*   [`Doc::preDelete()`][docpreDelete],
*   [`Doc::postDelete()`][docpostDelete],
*   [`Doc::undelete()`][undelete].

<!-- links -->
[docstore]:         #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[docpostcreated]:   #core-ref:b8f80e6b-a374-4bf4-bc76-47290cd69c45 "Hameçon Doc::postCreated()"
[docpoststore]:     #core-ref:99520a31-0aef-4bc6-b20a-114737059d17 "Hameçon Doc::postStore()"
[docprestore]:      #core-ref:3517da95-82fe-4adb-8bc4-ef49ca55edb0 "Hameçon Doc::preStore()"
[docprecreated]:    #core-ref:e85aa9d4-5e62-4a60-9d1c-f60433301747 "Hameçon Doc::preCreated()"
[docprerefresh]:    #core-ref:580d6be1-6b6a-439b-abd7-34b26cfaf2e5 "Hameçon Doc::preRefresh()"
[docpostrefresh]:   #core-ref:9352c534-3691-41e3-b293-599db8e9a4fd "Hameçon Doc::postRefresh()"
[docpreimport]:     #core-ref:adb6ba8b-15c4-42d3-97dc-1da16c2112ae "Hameçon Doc::preImport()"
[docpostimport]:    #core-ref:9de7e922-150a-416b-b846-b6e195bf0921 "Hameçon Doc::postImport()"
[docpreundelete]:   #core-ref:6ec8e3bc-90d2-4577-9152-a1a1f9341751  "Hameçon Doc::preUndelete()"
[docpostundelete]:  #core-ref:7d851f54-d167-4ecd-bbec-d3670023cc36  "Hameçon Doc::postUndelete()"
[undelete]:         #core-ref:e48b02c7-c684-4f71-a731-ac92064d13ae
[docpostDelete]:    #core-ref:8db8e5e0-b15e-4cfe-a891-66850df15b1e "Hameçon Doc::postDelete()"
[docpreDelete]:     #core-ref:dca7b2bd-fc69-4f6f-ab12-fb0de91dce8c "Hameçon Doc::preDelete()"
[searchdoc]:        #core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255
[docprop]:          #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353
[revision]:         #core-ref:8c947ef6-8da5-4fca-93cf-897c81ebb53d