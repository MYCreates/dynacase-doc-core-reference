# Mise à jour d'attribut sur des collections de documents {#core-ref:c28bea37-1f15-4157-aa79-40b5181d53f5}
## Présentation {#core-ref:abc4b8eb-4c82-4dcd-9231-127273e5141d}

Afin de modifier une valeur d'attribut sur un ensemble de document vous pouvez
utiliser une itération sur la liste en question comme le montre l'exemple ci-
dessous

    [php]
    $s = new \SearchDoc('', 'TST_MYFAMILY');
    $s->setObjectReturn();
    $s->setOrder('initid');
    $s->setSlice(1000);
    $dl=$s->search()->getDocumentList();
    foreach ($dl as $doc) {
        $doc->setValue('tst_example','my new value');
        $err=$doc->store();
    }

Plus le nombre de documents est important, plus le temps de modification sera
conséquent. L'utilisateur ayant déclenché ce programme risque de perdre
patience. 

La classe `UpdateAttribute` permet de réaliser un traitement équivalent
de manière optimisée.

    [php]
    $s = new \SearchDoc('', 'TST_MYFAMILY');
    $s->setObjectReturn();
    $s->setOrder('initid');
    $s->setSlice(1000);
    $dl=$s->search()->getDocumentList();
    $ua = new \UpdateAttribute();
    $ua->useCollection($dl);
    $ua->setValue('tst_example','my new value');
    $err=$ua->getError();

Ceci ne réalise pas tout à fait la même action. En effet dans la version
optimisée les post-traitement spécifiques et internes ne sont pas réalisés. Cela
veut dire que le titre, le profil, les attributs calculés ne sont pas mis à
jour. 

En contrepartie, le temps de traitement est bien moindre. Sur une mise à
jour "classique" pour un temps de réponse d'environ 45 secondes pour 1000
documents, la version optimisée réalise le traitement en moins de 2 secondes.

## Paramétrage général {#core-ref:866aea9b-fb97-4542-9044-50aa113b6d8c}
### Réviser les documents {#core-ref:d7f080be-6980-4f43-9982-3c1f297fa9bd}

Les documents peuvent être révisés avant la modification. Cela est indiqué par
la méthode `::addRevision()`.

    [php]
    $ua = new \UpdateAttribute();
    $ua->useCollection($dl);
    $ua->addRevision('my revision');
    $ua->setValue('tst_example','my new value');

La révision est faite sur les documents qui seront modifiés. Dans le cas du
setValue si la valeur est déjà celle que l'on veut mettre, la révision ne sera
pas effectuée. De manière générale si le document ne subit pas de modification,
il ne sera pas révisé. 

L'activation de ce paramètre va entraîner un temps de réponse plus important car
cela entraîne de nombreuses écritures supplémentaires pour les révisions.

### Recalculer le profil {#core-ref:c72af82d-dd5f-4390-b2ab-f14366e8a0fb}

Si vos documents sont lié à un profil dynamique et que la modification est faite
sur un attribut lié au profil, vous devez ajouter l'appel à la méthode
`::useProfileUpdating()`.

    [php]
    $ua = new \UpdateAttribute();
    $ua->useCollection($dl);
    $ua->useProfileUpdating();
    $ua->setValue('tst_redactor','MY_NEWREDACTOR');

### Ajouter un commentaire d'historique {#core-ref:3055b094-e482-47f7-a228-755fb42fb5e9}

Un commentaire spécifique, avec la méthode `::addHistoryComment()` peut être
ajouté dans l'historique sur chacun des documents. À la différence de la
révision, il est appliqué sur tous les documents qu'il soit changé ou non.

    [php]
    $ua = new \UpdateAttribute();
    $ua->useCollection($dl);
    $ua->addHistoryComment("my message");
    $ua->setValue('tst_example','my new value');

### Activer le mode transactionnel {#core-ref:424dcc1e-7f83-4b18-a104-2213aac1ade5}

Le mode transactionnel, permet d'éviter de faire un traitement partiel en cas
d'erreur. Par défaut ce mode n'est pas activé.

    [php]
    $ua = new \UpdateAttribute();
    $ua->useCollection($dl);
    $ua->useTransaction(true);
    $ua->setValue('tst_example','my new value');

S'il y a une exception levée ou si `::getError()` n'est pas vide le traitement
sera abandonné.

### Récupérer les statuts {#core-ref:214aee6b-cec0-4cb1-86c6-341e094a00e0}

Lorsque le traitement est fini, le statut de chacun des documents est donné par
la méthode `::getResults()`. Cela donne une table avec pour chacun des éléments,
des indications sur ce qui a été fait, l'index du tableau est l'initid du
document.

* [`changed`] => est à vrai si le document a subit une modification
* [`revisionError`] => message d'erreur de la révision (si révision)
* [`profilingError`] => message d'erreur du recalcul de profil
* [`profiled`] => est à vrai si le document a été reprofilé
* [`revised`] => est à vrai si le document a été révisé
* [`historyUpdated`] => est à vrai si l'historique a été mis à jour

    [php]
    $ua = new \UpdateAttribute();
    $ua->useCollection($dl);
    $ua->setValue('tst_example','my new value');
    $status=$ua->getResults();
    $changed=$unChanged=0;
    foreach ($status as $initid=>$aStatus) {
        if ($aStatus->changed) $changed++;
        else $unChanged++;
    }
    printf("%d document changed, %d document unchanged\n", $changed,$unChanged);

### Estimation des temps de réponse {#core-ref:08e083cc-9858-445d-807b-a2da12a4e091}

Ces temps sont donnés à titre indicatif pour le traitement de 1000 documents.
Ces temps peuvent fluctuer en fonction de votre configuration serveur et de la
configuration de la famille.

|        Pour 1000 documents         | Temps en secondes |
| ---------------------------------- | ----------------- |
| setValue par boucle traditionnelle | 45,64             |
| setValue seul                      | 1,27              |
| setValue + revision                | 68,83             |
| setValue + profiling               | 1,84              |
| setValue + historique              | 1,41              |
| setValue + revision + profiling    | 73,30             |
|                                    |                   |


## Fonctions de modifications {#core-ref:c72499ba-3d1c-4df2-854d-f1761368e48a}

Les quatre fonctions de modification permettent de réaliser des modifications
sur un attribut en particulier pour l'ensemble des documents donné.

### setValue {#core-ref:5769c462-24db-4a22-8d7b-49daebfe2df9}

La méthode `::setValue()` permet de changer de manière systématique la valeur
d'un attribut. Si la valeur d'un document est déjà celle choisie, alors le
document n'est pas modifié.

Si l'attribut est dans un tableau, c'est toute la colonne qui est modifiée. Il
est possible d'indiquer un tableau de valeur dans la cas d'un attribut multiple
ou d'un attribut qui est dans un tableau.

### replaceValue {#core-ref:2d3812af-3f59-4a30-b09b-3ff9794d2db6}

La méthode `::replaceValue()` permet de changer une valeur par une autre. 

Si la valeur à changer n'est pas présente, le document ne sera pas changé.

Pour les attributs multiples, le test de remplacement se fait sur chacune des
valeurs du multiple. De même, pour les multiples dans les tableaux, le test de
remplacement se fera sur les valeurs finales.

    [php]
    $ua = new \UpdateAttribute();
    $ua->useCollection($dl);
    $ua->replaceValue('tst_example','my old value','my new value');

### removeValue {#core-ref:f5ae9238-ea6f-4a34-951c-c934ba1444a7}

La méthode `::removeValue()` permet de supprimer une certaine valeur d'un
attribut. Cela ne permet pas de supprimer la valeur de l'attribut de manière
systématique. Si la valeur à supprimer n'est pas présente le document ne sera
pas changé.

Comme pour `::replaceValue()`, pour les attribut multiple, le test de
suppression se fait chacune des valeurs du multiple.

    [php]
    $ua = new \UpdateAttribute();
    $ua->useCollection($dl);
    $ua->removeValue('tst_example','value to remove');

Lorsque l'attribut est dans un tableau, cela laissera un vide dans le tableau où
la valeur se située.

### addValue {#core-ref:b8ca86ca-1cfe-4122-952b-c8521dd43a96}

La méthode `::addValue()` ne fonctionne qu'avec les attributs "multiples". Elle
permet d'ajouter une nouvelle valeur à la liste des valeurs de l'attribut. Si
l'attribut est déclaré multiple alors seuls les documents n'ayant pas encore
cette valeur seront changés. Si c'est un attribut simple qui est dans un tableau
alors une nouvelle rangée sera ajoutée au tableau.

Si c'est un attribut multiple qui est dans un tableau alors la valeur sera
ajoutée à tous les éléments du multiple.

    [php]
    $ua = new \UpdateAttribute();
    $ua->useCollection($dl);
    $ua->addValue('tst_multiple','value to add');

## Éxécution en tâche de fond {#core-ref:b491ee22-cd34-4c04-8483-c0d1d80ddcd7}

Comme ces tâches peuvent être longues, il est possible de les lancer en tâche de
fond. Cela est fait en remplaçant l'appel des fonctions de modification par leur
équivalent en tâche de fond.

* `::bgSetValue()`
* `::bgReplaceValue()`
* `::bgRemoveValue()`
* `::bgAddValue()`

    [php]
    $ua = new \UpdateAttribute();
    $ua->useCollection($dl);
    $fileStatus=$ua->bgAddValue('tst_multiple','value to add');

À la différence des fonctions lancées en synchrone, cette fonction retourne un
chemin vers un fichier qui sera mis à jour au fur et à mesure de l'avancement
par le processus lancé en tâche de fond. L'exploitation de ce fichier se fait
via la classe UpdateAttributeStatus. Elle permet de contrôler l'avancement de la
tâche.

    [php]
    $sua= new UpdateAttributeStatus($statusFile);
    while (! $sua->isFinished()) {
        print_r($sua->getLastMessage());
        sleep(1);
    }
    if ($err=$sua->getError()) {
        print "Process failed : ".$err;
    } else {
        $status=$sua->getResults();
        $changed=$unChanged=0;
    
        foreach ($status as $initid=>$aStatus) {
            if ($aStatus->changed) $changed++;
            else $unChanged++;
        }
        printf("%d document changed, %d document unchanged\n", $changed,$unChanged);
    }

La méthode `::getLastMessage()` retourne un objet `UpdateAttributeStatusLine`
avec les informations suivantes :

* `date` : date d'exécution (format ISO 8601)
* `processCode` : nom de la méthode utilisée
* `message` : message d'information

Extrait du fichier de status :

    2012-06-28T16:16:30 setValue BEGIN
    2012-06-28T16:16:30 setValue traitement
    [219656,220655,220656,220657,220658,220659,220660,220661,220662,220663,220664,22066
    5,220666,220667,220668,220669,220670,22067
    1,220672,220673,220674,220675,220676,220677,220678,220679,220680,220681]
    2012-06-28T16:16:30 executeRevision traitement des révisions pour 28 documents
    2012-06-28T16:16:32 executeRevision 10/28 révisions effectuées
    2012-06-28T16:16:34 executeRevision 20/28 révisions effectuées
    2012-06-28T16:16:35 executeRevision 28/28 révisions effectuées
    2012-06-28T16:16:35 executeRevision traitement des révisions effectué
    2012-06-28T16:16:35 executeSetValue traitement d'affectation de valeur pour 28
    documents
    2012-06-28T16:16:35 executeSetValue argument tst_redactor=>1181
    2012-06-28T16:16:35 executeSetValue 28 documents mis à jour
    2012-06-28T16:16:35 executeProfiling 10/28 mise à jour de profils effectués
    2012-06-28T16:16:36 executeProfiling 20/28 mise à jour de profils effectués
    2012-06-28T16:16:36 executeProfiling 28/28 mise à jour de profils effectués
    ...
    2012-06-28T16:16:36 setValue END