# Action::addLogMsg {#core-ref:1e4c336f-f2af-462b-86d5-938f6b385b79}

<div class="short-description">
Envoi un message sur la console web du navigateur
</div>


## Description  {#core-ref:4a75a71b-c915-42ee-8ab3-71d030cdb92a}


    [php]
    void addLogMsg ( string $msg, $cut = 0 )

Le message est affiché dans la [console][consolelog] du navigateur.

Si plusieurs messages sont envoyés, ils sont affichés chacun séparément dans la
console. Chaque message est préfixé par l'heure (heure et minute) de l'appel à
la fonction.

## Avertissements {#core-ref:f43b71f6-6cbf-4925-9298-eb57753daf99}

N/A

## Liste des paramètres  {#core-ref:cfb6218d-0682-407f-be3a-12466cdd1ab4}

(string) `msg`
:   Texte brut (pas de html) à afficher

(string) `cut`
:   Taille limite du texte, si `$cut` est strictement positif le texte est 
    tronqué au delà de cette limite


## Valeur de Retour  {#core-ref:3614358c-b2fd-4d4f-808f-c46219314b3f}

Aucun.

## Erreurs / Exceptions  {#core-ref:2bcf4b71-c0ca-4ef8-ae7c-47412d80e4b4}

Aucune.

## Historique  {#core-ref:a786823c-cff4-4174-bfc9-c106efdd9cdb}

N/A

## Exemples {#core-ref:0f035646-fbfd-43e1-9efb-141f8e1dd754}


    [php]
    $err = $action->addLogMsg('Mon premier message');

Affichera sur la console web :

    09:20 Mon premier message


## Notes  {#core-ref:fb14a718-03cb-4e6b-ba9b-249e54a32a6d}

Aucune.

## Voir aussi  {#core-ref:1048c987-68d8-4c5d-a49e-f43c4f9829c2}

N/A

<!-- links -->
[consolelog]:        https://developer.mozilla.org/fr/docs/Web/API/Console/log "MDN: window.console.log"