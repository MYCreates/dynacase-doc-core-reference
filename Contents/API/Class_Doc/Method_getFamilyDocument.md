# Doc::getFamilyDocument() {#core-ref:ec3fb8c2-f834-470b-a1af-0d4445fe0b6b}

<div class="short-description" markdown="1">

La méthode `getFamilyDocument` permet d'obtenir le document [famille][family] du
document courant.

</div>

## Description {#core-ref:990c503e-bfba-4821-be83-d9271c71499b}

    [php]
    DocFam|null getFamilyDocument ( )

La méthode `getFamilyDocument` permet d'obtenir l'objet [`DocFam`][class_DocFam]
de la [famille][family] du document courant.

### Avertissements {#core-ref:aa87b1e2-fb39-4416-a321-62f683a1157e}

Aucun.

## Liste des paramètres {#core-ref:3bf4f65e-b6b7-4bdc-93eb-b8ea08830f70}

Aucun.

## Valeur de retour {#core-ref:09e2f7cd-4796-4486-8b5f-bceecb563c7c}

La méthode retourne l'objet [`DocFam`][class_docfam] de la [famille][family] du
document courant ou la valeur `null` en cas d'erreur.

## Erreurs / Exceptions {#core-ref:683bc267-18d4-448d-8e1d-c2093dd8fe93}

La méthode retourne la valeur `null` s'il y a une erreur d'inclusion du fichier
PHP de déclaration de la famille du document courant.

## Historique {#core-ref:901cc67f-60a2-47a1-a884-d688045025e6}

### Release 3.2.5 {#core-ref:994eff3e-49b8-455e-93d7-8a807b361b47}

La méthode `getFamilyDocument` remplace la méthode précédemment nommée
`getFamDoc`.

L'utilisation de `getFamDoc` est obsolète depuis la version 3.2.5 de
dynacase-core.

### Release 3.3.0 {#core-ref:994eff3e-49b8-455e-93d7-8a807b361b47}

Si le document n'a pas de famille, null est retourné à la place d'un object
`DocFam` non initialisé.

Ceci concerne les documents "Famille" qui n'hérite pas d'une autre famille.

## Exemples {#core-ref:7b1d96d4-3e99-4c76-91ce-0be187c75eda}

- Exemple #1

    [php]
    $fam = $doc->getFamilyDocument();
    
    printf("Le document '%s' est issue de la famille '%s'.",
        $doc->getTitle(),
        $fam->getTitle()
    );

## Notes {#core-ref:b81700a6-1e2a-42ab-bb97-20713c247ba2}

Aucune.

## Voir aussi {#core-ref:19eb8e3a-cfc2-4f47-87b8-e91d11280882}

<!-- links -->
[family]: #core-ref:3ca3aeef-22a0-4e91-ad8b-357f2d05e227
[class_DocFam]: #core-ref:840462f3-4ea0-4e62-ad20-43d2dbfb2f64
