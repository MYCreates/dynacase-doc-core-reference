# Dcp\DocManager\Cache::hasDocument()   {#core-ref:45b26670-f06a-4054-959f-dc4408346e22}
<div class="short-description">
Teste si un document est dans le cache
</div>


## Description  {#core-ref:1597cc78-3086-4cd3-a6cc-f8da0cd1f379}

    [php]
    static bool Dcp\DocManager\Cache::hasDocument ( Doc  &$document )

Vérifie si l'objet _document_ est dans la cache. Un document ne peut être mis
dans le cache que par la méthode [`::addDocument()`][addincache].

### Avertissements   {#core-ref:c36f949e-adff-4af4-8197-74b226a4d490}

Le test est réalisé sur l'objet lui-même. Il est possible d'avoir deux instances
du même document avec une instance dans le cache et une autre qui n'est pas dans
le cache.

## Liste des paramètres  {#core-ref:5f3cd5eb-c137-4098-83f3-823ffa35d205}
 
(Doc) `document`
:   Objet document à vérifier 


## Valeur de retour  {#core-ref:de43d1bf-ddbb-420f-8f7b-4e055ff3ac4a}

Retourne `true` si l'objet est présent dans le cache.
Retourne `false` s'il n'est pas présent.

## Erreurs / Exceptions {#core-ref:ca2acba0-ee43-4574-ac16-c73e3b0e91e5}

Exception `\Dcp\DocManager\Exception`,  si le document n'a pas
d'identificateur.

## Historique  {#core-ref:75994e9e-02c6-4b7f-9c02-a647f8de862b}

Aucun.

## Exemples {#core-ref:adf72dec-899c-4d04-9866-78fd2b5e56a5}

### Vérification simple {#core-ref:7a82dac3-2f0c-490f-9639-b15978acd245}

Dans cet exemple, on considère que le document `1234` est potentiellement déjà
dans le cache.

    [php]
    $myDoc=Dcp\DocManager::getDocument(1234);
    if (! Dcp\DocManager::cache()->hasDocument($myDoc)) {
        Dcp\DocManager::cache()->addDocument($myDoc);
    }

### Instance multiple {#core-ref:4c603c35-2786-4ca0-982c-7d516363c8b4}

On suppose que les 2 documents ne sont pas encore dans le cache.

    [php]
    $myDoc1=Dcp\DocManager::getDocument(1234);
    $myDoc2=Dcp\DocManager::getDocument(1234);
    Dcp\DocManager::cache()->addDocument($myDoc2);
    
    printf("Document myDoc1 is%scached\n",
            Dcp\DocManager::cache()->hasDocument($myDoc1)?"":" not " );
    printf("Document myDoc2 is%scached\n",
            Dcp\DocManager::cache()->hasDocument($myDoc2)?"":" not " );

Résultat :

    Document myDoc1 is not cached
    Document myDoc2 is cached

### Variante instance multiple {#core-ref:cc01db40-0bf7-4a40-a8f0-41785cfa0c0e}

On suppose que les 2 documents ne sont pas encore dans le cache.

    [php]
    $myDoc1=Dcp\DocManager::getDocument(1234);
    Dcp\DocManager::cache()->addDocument($myDoc1);
    $myDoc2=Dcp\DocManager::getDocument(1234);
    
    printf("Document myDoc1 is%scached\n",
            Dcp\DocManager::cache()->hasDocument($myDoc1)?" ":" not " );
    printf("Document myDoc2 is%scached\n",
            Dcp\DocManager::cache()->hasDocument($myDoc2)?" ":" not " );

Résultat :

    Document myDoc1 is cached
    Document myDoc2 is cached

Ici dans ce cas, les deux variables adressent le même document : `$myDoc1 === $myDoc2`.

## Notes  {#core-ref:a13c0565-1c6c-4020-af54-805d2998b853}

Aucunes.

## Voir aussi {#core-ref:76d75aa6-86fd-4cde-a1a8-ef7a96904621}

*   [`Dcp\DocManager::getDocument`][getdocument]
*   [`Dcp\DocManager\Cache::addDocument`][addincache]

<!-- links -->
[getdocument]:      #core-ref:dfa0762f-6ff3-4349-bd21-6442740d9dcc
[addincache]:       #core-ref:15d6a036-3b6e-4dbd-a0fe-361b925e6186