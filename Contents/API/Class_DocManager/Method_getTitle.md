# Dcp\DocManager::getTitle()  {#core-ref:5d740af7-1bab-4957-82c5-1c6942de35d3}

<div class="short-description">
Retourne le titre d'un document
</div>


## Description  {#core-ref:85ce684f-f61c-4760-885e-23da99570e50}

    [php]
    static string Dcp\DocManager::getTitle ( int|string $documentIdentifier,
                                                   bool $latest=true)

Retourne le titre d'un document.

### Avertissements  {#core-ref:1ad6c683-43bb-4f0c-a02f-c54dce14ab82}

Cette méthode retourne le titre **brut**. Il peut différer du résultat de la
méthode [`Doc::getTitle()`][gettitle] si cette dernière implémente un [titre
dynamique][getcustomtitle] fonction de paramètres externes au document.

Cette méthode n'utilise pas le cache. Le titre est issu de la base de données.

## Liste des paramètres  {#core-ref:1f2e5e3c-59c4-4058-b785-5a4161888a46}

(int|string) `documentIdentifier`
:   Identifiant numérique du document ou nom logique du document.


(bool) `latest` 
:   Utilisable que pour un identifiant numérique. Indique si on veut la
    révision précise correspondant à l'identifiant ou la dernière révision.

## Valeur de retour  {#core-ref:b18d86b5-b8ad-4b29-9595-8a0e537dc836}

Le titre brut du document.
Pour les documents familles, le titre retourné est localisé.
Si l'identifiant ne correspond à aucun document `null` est retourné.

## Erreurs / Exceptions  {#core-ref:7aca59ca-12b5-4d05-b32c-7bae0ad28dec}

Exception `\Dcp\DocManager\Exception`, si l'identifiant n'est pas
syntaxiquement valide. L'identifiant doit être un nombre positif ou une chaîne
de caractère non vide.


## Historique  {#core-ref:78e0f35c-513d-4286-8ce3-0bf546d88be8}

Remplace la fonction `getDocTitle`.

## Exemples  {#core-ref:e1ab2b65-129b-4636-89ac-1774cd777eeb}

    [php]
    $myTitle=\Dcp\DocManager::getTitle(1234);
    

## Notes  {#core-ref:dbf0144e-d788-4ab8-bb75-8f668cfcd252}

Cette méthode est plus rapide que l'accès au titre à partir de
`DocManager::getDocument()` car elle ne charge pas l'objet Document en mémoire.

## Voir aussi  {#core-ref:331e61ca-cca0-48d6-b3c4-fafe21388363}

*   [`DocManager::getDocument()`][getdocument]
*   [`DocManager::getRawDocument()`][getrawdocument]

<!-- links -->
[getrawdocument]:   #core-ref:27f42abc-23c2-43c7-9a28-cfd32250632c
[getdocument]:      #core-ref:dfa0762f-6ff3-4349-bd21-6442740d9dcc
[searchdoc]:        #core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255
[doclist]:          #core-ref:23c71c28-dbce-4d34-819a-50d5bc4a38c3
[DocClass]:         #core-ref:1d557fb4-4eca-4ab8-a334-974fe563ddd2
[docstore]:         #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[gettitle]:         #core-ref:84011cc8-2aec-4f39-81f0-c7ae803e4913
[getcustomtitle]:   #core-ref:3c5ff78d-c080-48fb-a293-9736ed4e95b8