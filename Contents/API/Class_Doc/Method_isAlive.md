# Doc::isAlive() {#core-ref:b791d14e-800a-4b3b-bee7-41e271a8087e}

<div class="short-description" markdown="1">

La méthode `isAlive` permet de savoir si un document est *vivant* 
(s'il existe et n'a pas été supprimé).

</div>

## Description {#core-ref:0665bdd5-0745-4552-b831-bb68a5e5c8a7}

    [php]
    bool isAlive ( void )

La méthode `isAlive` permet de savoir si un document est *vivant*.

Un document est *vivant* si :

* le document est stocké en base de données ;
* la propriété [`doctype`][doc_props] est différente de `Z`.

### Avertissements {#core-ref:16a366b1-5401-48c1-981b-fb4ba4fca2d7}

Aucun.

## Liste des paramètres {#core-ref:9a813f0b-6472-4a66-a32f-951f375e4ddf}

Aucune.

## Valeur de retour {#core-ref:9cfe5dd3-4164-4542-98e9-6eafaed321fe}

La méthode retourne `true` si le document est *vivant* ou `false` dans le
cas contraire.

## Erreurs / Exceptions {#core-ref:f97afb28-7964-49a0-99f5-be9cfabe0e3d}

Aucune.

## Historique {#core-ref:55fa31f6-e447-4366-afce-ff98bcaada15}

Aucun.

## Exemples {#core-ref:c0e55c88-c23a-41e7-a72c-3aeb2ebb1656}

- Exemple #1

    [php]
    $facture = \Dcp\DocManager::getDocument( $id);
    if (!$facture->isAlive()) {
        throw new Exception(
            sprintf("Le document avec l'identifiant '%d' n'existe pas ou a été supprimé.", $id)
        );
    }

- Exemple #2

    [php]
    /*
     * $facture est un objet qui n'existe qu'en mémoire,
     * par conséquent, isAlive() retourne `false`
     */
    $facture = \Dcp\DocManager::createDocument('FACTURE');
    var_dump($facture->isAlive());
    
    /*
     * Une fois l'objet sauvegardé en base de données
     * isAlive() retourne alors `true`
     */
    $facture->store();
    var_dump($facture->isAlive());

Résultat :

    bool(false)
    bool(true)

## Notes {#core-ref:7ab231a2-7c0e-4231-a170-b22d9d030547}

Aucune.

## Voir aussi {#core-ref:c5ee1c0e-db1a-4b82-a2ac-5576f871992d}

- Propriété [`doctype`][doc_props]
- Méthode `DbObj::isAffected`

<!-- links -->
[doc_props]: #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353
