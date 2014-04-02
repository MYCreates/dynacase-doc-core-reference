# Doc::undelete() {#core-ref:e48b02c7-c684-4f71-a731-ac92064d13ae}

<div class="short-description" markdown="1">
Méthode utilisée pour restaurer un document supprimé.
</div>

## Description {#core-ref:3d48abbb-635d-46b6-b313-3898fbf5fc77}

    [php]
    string undelete (  )

Cette méthode permet d'annuler la suppression d'un document.

### Avertissements {#core-ref:d248cd25-e7e3-459c-a710-bf4f894d36a9}

Cette méthode ne permet pas de restaurer un document qui a été [supprimé
physiquement][delete].

## Liste des paramètres {#core-ref:86b6db28-221e-4e95-bcb8-eddcdc272dd6}

Aucun.

## Valeur de retour {#core-ref:a48bf20b-958e-47d3-ab00-91a1afec918f}

Message d'erreur. Si la chaîne retournée est non nulle, le message indique
l'erreur qui a interdit la restauration.

## Erreurs / Exceptions {#core-ref:ce3ef8e2-fa19-4382-b9c8-3e97fd0a3409}

Une erreur est retournée si :

*   Le document n'existe pas ou plus (physiquement supprimé)
*   Le document n'est pas supprimé
*   L'utilisateur n'a pas le droit `delete` sur le document
*   L'hameçon [`preUndelete()`][docpreundelete] retourne une erreur.

## Historique {#core-ref:d22072c6-ef5f-4709-aaa4-e979ea16bfc0}

Anciennement `Doc::revive()`.

## Exemples {#core-ref:cb9a4d38-5068-4626-85cf-3d13fb1d0571}

Restauration du document référencé par l'identificateur `$documentId`.

    [php]
    $doc=\Dcp\DocManager::getDocument( $documentId);
    if ($doc && !$doc->isAlive()) {
        $err=$doc->undelete();
        // maintenant le document est vivant
        if ($err == "") {
            printf("Restauration %s [%d]", $doc->getTitle(), $doc->id);
        }
    }

## Notes {#core-ref:01ae0cad-1485-4c4f-82ff-06fb0045d67a}

Une entrée dans l'historique est enregistrée à chaque restauration.

## Voir aussi {#core-ref:2a3597d1-75fc-48a8-8a30-ff351bdee10f}


*   [`Doc::postUndelete()`][docpostundelete],
*   [`Doc::preUndelete()`][docpreundelete],
*   [`Doc::delete()`][delete].

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
[docpreundelete]:   #core-ref:6ec8e3bc-90d2-4577-9152-a1a1f9341751 "Hameçon Doc::preUndelete()"
[docpostundelete]:  #core-ref:7d851f54-d167-4ecd-bbec-d3670023cc36 "Hameçon Doc::postUndelete()"
[undelete]:         #core-ref:e48b02c7-c684-4f71-a731-ac92064d13ae
[delete]:           #core-ref:c4372b13-c132-4148-9487-de2b7614d497