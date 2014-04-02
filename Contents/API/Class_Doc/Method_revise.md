# Doc::revise() {#core-ref:882e3730-0483-4dbc-9b9d-0d0b5cc31d38}

<div class="short-description"> 

Cette méthode crée une révision du document courant. Cela permet de garder une
version figée du document pour historique.

</div>

## Description {#core-ref:acc2155b-e52c-46a7-afdd-0161848aa343}

    [php]
    string revise ( string $comment = '' )

Le document courant est enregistré en base et est figé. Sa propriété `locked`
vaut alors `-1`. La méthode `Doc::isFixed()` permet de savoir si un document est
figé.

Une copie du document est créée. Un nouvel identifiant est créé et est affecté à
la propriété `id`. Le document conserve le même identifiant initial (propriété
`initid`) et son numéro de révision (propriété `revision`) est incrémenté de un.

L'objet courant devient le nouveau document avec un nouvel identifiant.

### Avertissements {#core-ref:7c989da3-6457-46f6-8814-8c8c4bd1557d}

Un document figé ne peut pas être révisé.
Un document qui n'a pas d'identifiant ne peut pas être révisé.

## Liste des paramètres {#core-ref:89c75017-4c9d-40c5-8994-e22e742ab011}

(string) `comment`
:   Si `comment` n'est pas vide, le message sera ajouté à l'historique avant la
    révision.

## Valeur de retour {#core-ref:8a5fd585-05b8-41de-9090-137119d2ed02}

Message d'erreur : Si un message non vide est retourné alors la révision a été
abandonnée (la raison est donnée par le message d'erreur).

## Erreurs / Exceptions {#core-ref:9352bbb3-7d24-457b-ae85-15a0b5cd83e5}

Les causes d'erreur sont :

*   document figé,
*   problème de droits : la révision nécessite le droit `edit` sur le document
    et le droit `create` sur la famille du document,
*   retour d'erreur de l'hameçon [preRevise()][docprerevise].

## Historique {#core-ref:b80227eb-4f01-430a-9882-87b53f2413dc}

Cette méthode était anciennement nommée `addRevision`.

## Exemples {#core-ref:7870434a-e3f7-4357-90c7-7b6d7602b0a4}

Exemple de création d'une révision.

    [php]
    /** @var \Dcp\Family\MyFamily */
    $d = \Dcp\DocManager::getDocument( "MY_DOCUMENT");
    if ($d->isAlive()) {
        printf("Identifiant %d\n", $d->id);
        $err=$d->revise();
        if (empty($err)) {
            printf("Nouvel identifiant %d", $d->id);
        } else {
            printf("Erreur de révision %s", $err);
        }
    }

## Notes {#core-ref:4d4c5453-9f9c-4145-8109-e0d0771c8780}

Lors d'un ajout de documents les hameçons suivants sont lancés :

1.  [`Doc::preRevise()`][docprerevise]  
1.  Fige le document et enregistrement en base
1.  [`Doc::preCreated()`][docprecreated]  
1.  Copie du document et création nouvel identifiant
1.  [`Doc::postCreated()`][docpostcreated]
1.  [`Doc::postRevise()`][docpostrevise]  

### Hameçon `preRevise` {#core-ref:936ffcc4-35ff-4bab-b3c9-e67f0f501421}

L'hameçon `preRevise` a pour rôle de valider les conditions de révision du
document. Si cette méthode retourne une chaîne de caractère non-vide alors elle
est considérée comme un retour d'erreur et la révision n'est pas effectuée.
Ce message est retourné par la méthode `Doc::revise()`.

### Hameçon `postRevise` {#core-ref:4fd0befd-c587-4259-835b-3f4fa7c24e4b}

L'hameçon `postRevise` a pour rôle de réaliser un post-traitement après la
révision. Cette méthode utilise la nouvelle révision. Si cette méthode retourne
une chaîne de caractères non-vide alors elle est considérée comme un message
d'erreur. Ce message est enregistré dans l'historique du document.

## Voir aussi {#core-ref:68f970b7-6995-47c4-8d24-bf142e2ace1f}

*   [`Doc::store()`][docstore],
*   [`Doc::addHistoryEntry()`][docaddhisto],
*   [`Doc::disableEditControl()`][disableeditcontrol].

<!-- links -->
[docstore]:         #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[docpostcreated]:   #core-ref:b8f80e6b-a374-4bf4-bc76-47290cd69c45 "Hameçon Doc::postCreated()"
[docpoststore]:     #core-ref:99520a31-0aef-4bc6-b20a-114737059d17 "Hameçon Doc::postStore()"
[docprestore]:      #core-ref:3517da95-82fe-4adb-8bc4-ef49ca55edb0 "Hameçon Doc::preStore()"
[docprecreated]:    #core-ref:e85aa9d4-5e62-4a60-9d1c-f60433301747 "Hameçon Doc::preCreated()"
[docprerefresh]:    #core-ref:580d6be1-6b6a-439b-abd7-34b26cfaf2e5 "Hameçon Doc::preRefresh()"
[docpostrefresh]:   #core-ref:9352c534-3691-41e3-b293-599db8e9a4fd "Hameçon Doc::postRefresh()"
[docprerevise]:    #core-ref:936ffcc4-35ff-4bab-b3c9-e67f0f501421 "Hameçon Doc::preRevise()"
[docpostrevise]:   #core-ref:4fd0befd-c587-4259-835b-3f4fa7c24e4b "Hameçon Doc::postRevise()"
[contraintes]:      #core-ref:7b41906b-f199-41a4-94df-33b9ad34153b
[docaddrevision]:   #core-ref:882e3730-0483-4dbc-9b9d-0d0b5cc31d38
[docaddhisto]:      #core-ref:2f1828bf-b312-4996-9a56-42ac79e7dbf3
[disableeditcontrol]: #core-ref:b9844f4b-3e5c-4bbe-98ae-e71a4ba17239
