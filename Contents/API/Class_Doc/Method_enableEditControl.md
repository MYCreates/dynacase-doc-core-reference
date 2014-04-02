# Doc::enableEditControl() {#core-ref:3c9aba8e-50a6-41b0-82e9-64f63085e5e5}

<div class="short-description" markdown="1">

Cette méthode permet de restaurer le contrôles des droits de modifications
précédemment suspendus par la méthode [`Doc::disableEditContol()`][disable].

</div>

## Description {#core-ref:e2298940-5207-4da3-9da3-03c10b678742}

    [php]
    void enableEditControl (  )


Cette méthode restaure le contrôle de droit de modification.

Si la méthode [`Doc::disableEditContol()`][disable] a été appelée plusieurs fois
consécutivement, il est nécessaire d'appeler autant de fois cette méthode pour
restaurer le contrôle.

### Avertissements {#core-ref:e0f746f6-5609-4e5f-a97a-b7f87230b881}

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

## Liste des paramètres {#core-ref:cf426dcb-8b2d-42fa-b8b7-bc2d3725dd15}

Aucun.

## Valeur de retour {#core-ref:ccab4601-73f4-4711-8d40-104a23653ed7}

void.

## Erreurs / Exceptions {#core-ref:c7597228-670f-43f3-8fd4-749667871099}

Aucune.

## Historique {#core-ref:5d5cc165-6d39-49a5-af20-45f9cb8f9e23}

Aucun.

## Exemples {#core-ref:8f329704-a195-4085-90a0-3d314a037118}

### Imbrication des suspensions de contrôle de modification. {#core-ref:4883120a-3e90-4f2a-9d59-da56dec4282d}

L'utilisateur courant n'a pas le droit `edit` sur le document n°1420.

    [php]
    function modifyBirthday(Doc &$doc) {
        $doc->disableEditControl();
        print "Suspension du contrôle\n";
        
        $err = $doc->setValue(\Dcp\AttributeIdentifiers\Zoo_animal::an_naissance,"2013-01-01");
        print "\tAffectation an_naissance\n";
        
        $doc->enableEditControl();
        print "Activation du contrôle\n";
        
        return $err;
    }
    
    function modifyNameAndBirthday(Doc &$doc) {
        $doc->disableEditControl();
        print "Suspension du contrôle\n";
        
        $err = $doc->setValue(\Dcp\AttributeIdentifiers\Zoo_animal::an_nom,"Helitor");
        print "\tAffectation an_nom\n";
        
        if (empty($err)) {
            $err=modifyBirthday($doc);
            if (empty($err)) {
                $err=$doc->store();  
                print "\tEnregistrement\n";
            }
        }
        
        $doc->enableEditControl();
        print "Activation du contrôle\n";
        
        return $err;
    }
    
    $doc=\Dcp\DocManager::getDocument( "1420");
    
    if ($doc->isAlive()) {
        $err = modifyNameAndBirthday($doc);
        if (empty($err)) {
            printf("Document \"%s\" a été enregistré\n", $doc->getTitle());
        } else {
            printf("Error: %s\n", $err);
        }
    } else {
        printf("Document non trouvé\n");
    }


Résultat :

    Suspension du contrôle
        Affectation an_nom
    Suspension du contrôle
        Affectation an_naissance
    Activation du contrôle
        Enregistrement
    Activation du contrôle
    Document "Helitor" a été enregistré


La première activation de contrôle n'est pas effective car deux suspensions ont
été réalisées précédemment. Seule la deuxième activation rétablit le contrôle.

## Notes {#core-ref:231294fa-a345-4107-9e09-c92c6c3f02ec}

Aucune.

## Voir aussi {#core-ref:fc65c668-d2cd-4725-b078-828f161197c6}

*   [`Doc::disableEditControl()`][disable].

<!-- links -->
[acldoc]:       #core-ref:a99dcc5f-f42f-4574-bbfa-d7bb0573c95d
[setvalue]:     #core-ref:febc397f-e629-4d47-955d-27cab8f4ed2f
[store]:        #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[revise]:       #core-ref:882e3730-0483-4dbc-9b9d-0d0b5cc31d38
[disable]:      #core-ref:b9844f4b-3e5c-4bbe-98ae-e71a4ba17239