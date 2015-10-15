# Action::addWarningMsg {#core-ref:4ee92978-bed2-4c2a-8e1a-04d37b1a3328}

<div class="short-description">
Envoi un message sur l'interface de l'utilisateur.
</div>


## Description  {#core-ref:d5c4c300-eb9a-4104-99b1-1026da722c9c}


    [php]
    void addWarningMsg ( string $msg )

Le message est affiché sous forme de fenêtre de dialogue si l'interface dispose
de la fonction [jquery ui dialog][jDialog] (ce qui est le cas d'une vue de
document). Sinon le message est affichée avec la fonction
[window.alert()][alert].

Si plusieurs messages sont envoyés, ils sont affichés dans la même fenêtre les
uns au dessous des autres.

## Avertissements {#core-ref:e0f72958-6330-4917-81a0-a30ec877b2b0}

N/A

## Liste des paramètres  {#core-ref:6e15fdea-5108-448d-a0f5-852423165e99}

(string) `msg`
:   Texte brut (pas de html) à afficher


## Valeur de Retour  {#core-ref:e859029f-0544-41e4-b646-c686291751d0}

Aucun.

## Erreurs / Exceptions  {#core-ref:73d37352-9660-4a88-8f2a-537f82d6259e}

Aucune.

## Historique  {#core-ref:c125b790-78cf-4ec9-8dfa-e345eece2548}

N/A

## Exemples {#core-ref:edd5062b-833f-46c4-bca5-5cd3f29355be}


    [php]
    $err = $action->addWarningMsg('Mon premier message');
    


## Notes  {#core-ref:b9be6746-1dee-4c4d-bdf5-6535e845fc88}

Les messages peuvent être capturés par la fenêtre parente si celle ci déclare la
fonction  `dcp.displayWarningMessage(messages)`. Cette fonction reçoit en
paramètre la liste des messages d'avertissement (`array`).

    [javascript]
    window.dcp = window. dcp || {};
    window.dcp.displayWarningMessage = function (messages) {
        for (var i=0;i<messages.length;i++) {
            // Do something with messages[i]
        }
    }

## Voir aussi  {#core-ref:1f032e2a-087f-4ea6-96de-6b77429318b7}

N/A

<!-- links -->
[jDialog]:      https://jqueryui.com/dialog/ "jQuery UI Dialog"
[alert]:        https://developer.mozilla.org/fr/docs/Web/API/Window/alert "MDN: window.alert"