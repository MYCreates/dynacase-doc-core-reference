# Dcp\DocManager::getRawDocument()  {#core-ref:27f42abc-23c2-43c7-9a28-cfd32250632c}

<div class="short-description">
Retourne les valeurs et les propriétés brutes d'un document.
</div>


## Description  {#core-ref:a3e267d7-1389-466d-b241-ab659b21d665}

    [php]
    static string[] Dcp\DocManager::getRawDocument (  int|string $documentIdentifier,
                                                            bool $latest=true )

Retourne un tableau de valeurs de document issus de la base de données.

### Avertissements  {#core-ref:d466656a-6f8f-4f53-9e6b-f29d150edae5}

Un accès à la base de donnée est systématiquement effectué.

Les droits d'accès ne sont pas vérifiés.

## Liste des paramètres  {#core-ref:b10e174c-07f3-4757-b240-b81d23611231}

(int|string) `documentIdentifier`
:   Identifiant numérique du document ou nom logique du document.

(bool) `latest` 

:   Utilisable que pour un identifiant numérique. Indique si on veut la
    révision précise correspondant à l'identifiant ou la dernière révision.
 

## Valeur de retour  {#core-ref:601f0e65-4f6f-401d-9d99-0de0ece4a808}

Tableau de valeurs brutes. L'index du tableau contient l'identifiant de la
propriétés ou de l'attribut. Tous les identifiants retournés sont en minuscules.

Si l'identifiant ne correspond à aucun document la valeur `null` est
retournée.

## Erreurs / Exceptions  {#core-ref:7dd04857-448e-46f9-95d4-190f35a90279}

Exception `\Dcp\DocManager\Exception`, si l'identifiant n'est pas
syntaxiquement valide. L'identifiant doit être un nombre positif ou une chaine
de caractère non vide.

## Historique  {#core-ref:4ec227eb-02ba-4e33-9265-37771c4c799d}

Remplace le fonction `getTDoc`.

## Exemples  {#core-ref:b2cd782e-e374-4870-a532-4989c21ae0fb}

### Accès par identifiant {#core-ref:e9e12924-473b-4562-8223-06eeae896b9a}

Identifiant numérique :
La document `1234` est de la famille `MY_FAMILY`.

    [php]
    use AMyFamily as \Dcp\AttributeIdentifiers\MyFamily
    $rawDoc=\Dcp\DocManager::getRawDocument(1234);
    if ($rawDoc !== null) {
        // le titre
        print $rawDoc["title"];
        // l'attribut my_number1
        print $rawDoc[AMyFamily::my_number1];
    }


## Notes  {#core-ref:6d9396ff-3ef1-4a85-a6fb-ba8b84afd460}

L'utilisation de cette méthode est plus rapide et consomme moins de mémoire que
[`DocManager::getDocument()`][getdocument] suivi de
[`Doc::getTitle()`][gettitle] mais peut donner un résultat différent dans le cas
où le titre est dynamique (usage de [`Doc::getCustomTitle()`][getcustomtitle]).

## Voir aussi  {#core-ref:973f27a8-c826-4261-a2f3-b055010ff0b1}

*   [`Dcp\DocManager::getDocument()`][getdocument]

<!-- links -->
[getcustomtitle]:   #core-ref:3c5ff78d-c080-48fb-a293-9736ed4e95b8
[gettitle]:         #core-ref:84011cc8-2aec-4f39-81f0-c7ae803e4913
[getdocument]:      #core-ref:dfa0762f-6ff3-4349-bd21-6442740d9dcc