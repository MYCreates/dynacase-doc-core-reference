# Action::getWarningMsg {#core-ref:507c31a2-8c95-4587-85bb-f87189f88402}

<div class="short-description">
Récupère les messages à afficher à l'utilisateur.
</div>

## Description  {#core-ref:68f1f131-05a8-42dc-9261-e0f0a3b2b5f2}

    [php]
    void getWarningMsg ()

Retourne un tableau de messages.

## Avertissements {#core-ref:fc879083-04d7-4990-801a-54d9391b0e86}

N/A

## Liste des paramètres  {#core-ref:5ffe7a19-919f-43e1-a77b-49915ff817dd}

Aucun


## Valeur de Retour  {#core-ref:fadf03a9-c8d2-41bf-8ed2-84b0d42cfd62}

Un tableau dans lequel chaque entrée est un message.

## Erreurs / Exceptions  {#core-ref:8004d55f-e474-40d1-a38e-88ecf253efcc}

Aucune.

## Historique  {#core-ref:379f610d-4aeb-47db-8e19-e25c97d3babe}

N/A

## Exemples {#core-ref:90257cb4-9d30-43a2-a31f-3388142534ff}


    [php]
    foreach( $action->getWarningMsg() as $msg )
    {
        //do something with the message
    }

## Notes  {#core-ref:eeaa5ff1-65f8-450e-92ca-9241f99e8f6d}

Les messages ainsi récupérés ne sont pas effacés de la session.
Il faut, pour cela, utiliser la méthode [Action::clearWarningMsg][core-ref:action::clearWarningMsg].

## Voir aussi  {#core-ref:3c7f3417-b9c0-4abd-ac1b-c36b297e6764}

-   [Action::addWarningMsg][core-ref:action::addWarningMsg]
-   [Action::clearWarningMsg][core-ref:action::clearWarningMsg]

<!-- links -->
[core-ref:action::addWarningMsg]: #core-ref:4ee92978-bed2-4c2a-8e1a-04d37b1a3328
[core-ref:action::clearWarningMsg]: #core-ref:24e1d741-daf8-40e5-95bb-86846e0a1a6a