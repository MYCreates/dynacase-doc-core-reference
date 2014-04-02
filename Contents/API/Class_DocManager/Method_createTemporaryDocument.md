# Dcp\DocManager::createTemporaryDocument()  {#core-ref:5e5cda73-398a-49fd-a14f-24ee877cf3fa}

<div class="short-description">
Créé un objet document temporaire.
</div>


## Description  {#core-ref:03ccb0bc-7b33-4f60-aed2-09b3435f15a5}

    [php]
    static Dcp\Family\Document createTemporaryDocument(
          int|string $familyIdentifier,
                bool $useDefaultValues`=true) 

Créé un objet document temporaire. La classe de l'objet retourné est
fonction de la famille du document. L'objet document n'a pas encore
d'identifiant. Son identifiant **temporaire** est donné lors de
l'enregistrement (`Doc::store()`).

Un identifiant temporaire a une durée de vie de au plus 24h. Cet identifiant
est réutilisable dès que la suppression du document temporaire a été effectuée
par le programme de nettoyage.

Un document temporaire n'a pas de droit.

### Avertissements  {#core-ref:0d9d3bb9-598c-4793-854d-828b23f0bc1a}

Le document n'est pas mis en cache.
Les droits ne sont pas vérifiés.

Les documents temporaires sont supprimés toutes les nuits.

## Liste des paramètres  {#core-ref:58dde8e5-a9e4-4766-8436-d2728bb06f98}

(int|string) `familyIdentifier`
:   Identifiant de la famille du document à créer

(bool) `useDefaultValues`
    Indique s'il faut initialiser les attributs avec leurs valeurs par défaut.


## Valeur de retour  {#core-ref:2524dec9-4ba2-478e-9210-99af8b13b6d6}

Retour un objet de la classe de la famille indiquée.


## Erreurs / Exceptions  {#core-ref:1e7af7c1-5263-48df-9d97-fb5443821b3c}

Exception `\Dcp\DocManager\Exception` :

*    Si l'identifiant de famille n'est pas syntaxiquement valide.
    L'identifiant doit être un nombre positif ou une  chaine de caractère non
    vide.
*   Si la famille n'existe pas.
*   Si le droit de créer n'est pas accordé (droit `create`). 



## Historique  {#core-ref:e2b8cb26-b643-46d4-988c-ab1b866af86c}

Remplace la fonction [`createTmpDoc`][createtmpdoc].

## Exemples  {#core-ref:08fba235-ab93-45dd-a99a-1cee3cc896cd}

    [php]
    use AMyFamily as \Dcp\AttributeIdentifiers\MyFamily
    use MyFamily as \Dcp\Family\MyFamily
    $doc=\Dcp\DocManager::createTemporaryDocument(MyFamily::familyName);
    $doc->setAttributeValue(AMyFamily::my_number1, 345);
    $doc->setAttributeValue(AMyFamily::my_number2, 654);
    $err=$doc->store();
    if (empty($err)) {
        printf("Nouveau document temporaire %s n°%d", $doc->getTitle(), $doc->id);
    }

## Notes  {#core-ref:d574a8f0-2b8d-4ae5-9db3-84f44b85e353}

Le document temporaire peut aussi servir pour réaliser des opérations sans pour
autant nécessiter un enregistrement en base de données.

## Voir aussi  {#core-ref:25cdaafb-9a4c-40ad-837d-2bc92171ab69}

*   [\Dcp\DocManager\createDocument][createdocument]

<!--links -->
[createdocument]:   #core-ref:2f5afd12-1db3-4c69-a0fa-4b7fb044b723
[createtmpdoc]:     #core-ref:6b745549-eb65-46f5-b0c1-5fa80661f1b7
