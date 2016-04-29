# Classe SearchDoc {#core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255}

La classe SearchDoc permet de chercher des documents sans faire directement du
SQL et connaître la structure de la base de données. Cette recherche est basée
sur la notion de [famille][famintro] et les structures associées ([attributs][attributintro] et héritage).

**Attention** : La classe SearchDoc possède deux modes de fonctionnement :

* [résultat brut][resultatBrut],
* [résultat objet documentaire][resultatDocumentaire].

## Constructeur {#core-ref:631a292f-6012-4881-bd5c-a264d3830846}

    [php]
    new SearchDoc ( '', "FAM_LOGICAL_NAME" )

### Liste des paramètres {#core-ref:76a0107d-cb21-4079-9f70-51d7651348ea}

(string) `dbaccess`
:   **Déprécié**. Utilisé pour indiquer les coordonnées de la base, la récupération
    des coordonnées est maintenant automatique, il faut passer une chaîne vide `""`.

(string|int) `famname`
:   Identifiant interne ou nom logique de la famille sur laquelle s'effectue la
    recherche.
    
    Il peut prendre quelques valeurs particulières qui ne sont pas des
    identifiants de famille :
    
    -   `0` : (valeur par défaut) : La recherche est alors effectuée sur
        l'ensemble des documents quelque soit leur famille,
    -   `-1` : la recherche est alors effectuée sur les
        [documents familles][docfam].

### Propriétés de la classe {#core-ref:72ce807f-a392-4579-ab46-d58a3d18c214}

(string|int) fromid
:   Identifiant interne ou nom logique de la famille sur laquelle s'effectue la
    recherche.

(string|int) dirid
:   Identifiant interne ou nom logique d'une collection dans laquelle s'effectue la
    recherche.

(bool) recursiveSearch (valeur par défaut `false`)
:   Uniquement applicable dans le cas d'une recherche avec dirid, si il est passé
    à `true` et que la collection de base contient d'autres collection alors la
    recherche est aussi effectuée dans les autres collections.

(int) folderRecursiveLevel
:   Uniquement applicable dans le cas d'une recherche avec dirid et recursiveSearch
    à `true`. Indique le niveau de récursivité qui va être appliqué.
    **Attention** : La recherche récursive ne protège pas contre les boucles 
    si le dossier A contient le dossier B et que le dossier B contient le dossier
    A alors A et B sont ouvert à tour de rôle.

(int) slice
:   Indique le nombre maximum de documents retournés par la recherche.

(int) start
:   Indique à partir de combien de documents trouvés commence le retour des
    documents.

(bool) only (valeur par défaut `false`)
:   Indique si la recherche est faite dans les familles et les sous-familles.  
    Si `true` la recherche n'est pas faite dans les sous familles.

(bool) distinct (valeur par défaut `false`)
:   Indique si les résultats retournés comprennent toutes les révisions trouvées
    ou pas (`false` indique que tous les résultats sont renvoyés). Cette 
    propriété n'a de sens que si `latest` est à `false`.

(string) trash (valeur par défaut `no`)
:   Indique si la recherche doit chercher aussi dans les documents supprimés.  
    La [propriété][docprop] `doctype` est à `Z` si le document est supprimé.  
    Valeurs possibles :  
    
    *   `no` : Les documents supprimés sont exclus,
    *   `also` : Les documents supprimés sont inclus,
    *   `only` : Les documents non supprimés sont exclus, seuls les supprimés 
    sont inclus.

(bool) latest (valeur par défaut `true`)
:   Indique si la recherche ne doit retourner que la dernière révision des 
    documents.

(int) userid
:   Indique l'identifiant système de l'utilisateur avec lequel se fait la 
    recherche.

### Avertissement {#core-ref:d264c169-c6fb-4706-8505-b537b16f9c15}

Les objets de la classe SearchDoc fonctionne en deux phases :

* Préparation de la requête : lors de cette phase on initialise la requête et on
fixe les différents éléments la constituants (filtre, jointure, etc.)
* Exécution de la recherche : lors de cette phase la recherche est exécutée et
on exploite les résultats.

Le passage de la phase de préparation à la phase d'exécution se fait via la 
méthode [`SearchDoc::search`][search].

### Exemples {#core-ref:b459e7e1-a283-4ee5-970d-7572872a438a}

Recherche sur tous les documents :

    [php]
    $searchDoc = new SearchDoc();
    $documents = $searchDoc->search();

Recherche sur toutes les familles :

    [php]
    $searchDoc = new SearchDoc("", -1);
    $documents = $searchDoc->search();

Recherche sur les documents de la famille IUSER :

    [php]
    $searchDoc = new SearchDoc("", "IUSER");
    $documents = $searchDoc->search();

### Voir aussi {#core-ref:09a8b94f-8912-49c0-b980-68dd217d07be}

[Recherche documentaire avancée][advancedSearchDoc].

<!-- links -->

[famintro]:         #core-ref:e263d44b-8357-4450-87bf-11cef8bafb24
[attributintro]:    #core-ref:97f8232d-a45e-45ce-b559-71c38855edec
[docfam]:           #core-ref:63fa0a57-efe6-4bf1-853d-0caa3b2f88f0
[pgOrderBy]:        http://www.postgresql.org/docs/9.3/static/sql-select.html#SQL-ORDERBY "Postgresql : ORDER BY"
[resultatBrut]:             #core-ref:4c508940-f5a0-40ee-a942-6372a95d112e
[resultatDocumentaire]:     #core-ref:84a293c0-0ea6-428c-8da6-f8cc46980d5b
[advancedSearchDoc]:        #core-ref:7291dea8-a2db-46be-8194-bc6f100cc467
[search]:                   #core-ref:6f5cc024-66e4-429e-9071-67d4523a8e08
[reset]:                    #core-ref:39efa6f3-4d70-452c-b14b-891adca3a56d
[docprop]:                  #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353