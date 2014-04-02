# Doc::disableEditControl() {#core-ref:b9844f4b-3e5c-4bbe-98ae-e71a4ba17239}

<div class="short-description" markdown="1">

Cette méthode permet aux méthodes de modification de ne pas tenir compte du
[droit][acldoc] de modification.

</div>

## Description {#core-ref:1a44a203-da04-4571-817c-803c0961adfb}

    [php]
    void disableEditControl (  )

Les méthodes suivantes ne vérifient plus le droit de modification (`edit`) :

*   [`Doc::setValue()`][setvalue],
*   [`Doc::store()`][store]
*   [`Doc::revise()`][revise]

La méthode de vérification de modification `Doc::canEdit()` ne vérifie plus le
droit `edit`, mais continue à vérifier les verrous.

### Avertissements {#core-ref:e95213e6-089a-447f-a76d-a4f171a1ab66}

Les appels successifs à `disableEditControl` se cumulent. Ainsi, si une méthode
fait appel à `disableEditControl` et n'appelle pas `enableEditControl`, toutes
les méthodes qui se déclenchent ensuite outrepasseront les privilèges de
l'utilisateur. Aussi, à moins de vouloir explicitement ce comportement, tout
appel à `disableEditControl` doit être suivi d'un appel à `enableEditControl`
avant que la fonction ne retourne, même en cas de retour anticipé à cause d'une
erreur. Notamment, le code suivant laisse les contrôles désactivés :

    [php]
    $doc = \Dcp\DocManager::getDocument( 1000);
    $doc->disableEditControl();
    $doc->disableEditControl();
    $doc->enableEditControl();

## Liste des paramètres {#core-ref:4e1be90b-157d-4386-9c08-f4194fe3cb4d}

Aucun.

## Valeur de retour {#core-ref:df536f36-804e-4fba-9e78-c0c2509885e0}

void.

## Erreurs / Exceptions {#core-ref:edbd07d9-710f-4bdd-ac58-42f6e774ef30}

Aucune.

## Historique {#core-ref:a7a6ddf6-d3d1-44ac-b770-13fa7dc11f43}

Aucun.

## Exemples {#core-ref:6bdca8c4-7c5c-4cc3-ab09-36c70ea2bd5f}

L'utilisateur courant n'a pas le droit `edit` sur le document n°1420.

### Sans suspension de contrôle de droit {#core-ref:8bf775f8-c835-4fc6-a8b2-cdb8fdc4e34c}

    [php]
    require_once("FDL/Class.Doc.php");
    $doc = \Dcp\DocManager::getDocument( "1420");
    if ($doc && $doc->isAlive()){
        $err1 = $doc->setValue(\Dcp\AttributeIdentifiers\Zoo_animal::an_nom,"Gastor");  
        $err2 = $doc->store();
        printf( "Erreur : [1:%s] - [2:%s]\n", $err1, $err2);
    } else {
        printf("Document non trouvé\n");
    }

Résultat :

    Erreur : [1:Pas de privilège edit pour le document Théodor [1420]] - [2:]

Attention, la méthode [`Doc::store()`][store] ne retourne pas d'erreur dans ce
cas, car l'affectation n'a pas eu lieu et comme aucune modification n'a été
détectée, l'enregistrement en base de données n'a pas eu lieu.

### Avec suspension de contrôle de droit sur l'affectation {#core-ref:0716b826-b5ed-4bb5-9b96-07fd48a99497}

    [php]
    require_once("FDL/Class.Doc.php");
    $doc = \Dcp\DocManager::getDocument( "1420");
    if ($doc && $doc->isAlive()){
        $doc->disableEditControl();
        $err1=$doc->setValue(\Dcp\AttributeIdentifiers\Zoo_animal::an_nom,"Gastor");  
        $doc->enableEditControl();
        $err2=$doc->store();
        printf( "Erreur : [1:%s] - [2:%s]\n", $err1, $err2);
    } else {
        printf("Document non trouvé\n");
    }

Résultat :

    Erreur : [1:] - [2:Pas de privilège edit pour le document Théodor [1420]]

### Avec suspension complète de contrôle de droit {#core-ref:7c6d43a8-fd07-4fc6-9906-8c4a7820dd70}

    [php]
    require_once("FDL/Class.Doc.php");
    $doc = \Dcp\DocManager::getDocument( "1420");
    if ($doc && $doc->isAlive()) 
        $doc->disableEditControl();
        $err1=$doc->setValue(\Dcp\AttributeIdentifiers\Zoo_animal::an_nom,"Gastor");  
        $err2=$doc->store();
        $doc->enableEditControl();
        printf( "Erreur : [1:%s] - [2:%s]\n", $err1, $err2);
    } else {
        printf("Document non trouvé\n");
    }

Résultat :

    Erreur : [1:] - [2:]


L'affectation ainsi que l'enregistrement ont bien pu être réalisés.

## Notes {#core-ref:c8372033-9534-492e-affb-93ce83b87826}

Aucune.

## Voir aussi {#core-ref:b4e58901-7c30-4c86-8369-14de31f5e71c}

*   [`Doc::enableEditControl()`][enable].

<!-- links -->
[acldoc]:       #core-ref:a99dcc5f-f42f-4574-bbfa-d7bb0573c95d
[setvalue]:     #core-ref:febc397f-e629-4d47-955d-27cab8f4ed2f
[store]:        #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[revise]:       #core-ref:882e3730-0483-4dbc-9b9d-0d0b5cc31d38
[enable]:       #core-ref:3c9aba8e-50a6-41b0-82e9-64f63085e5e5