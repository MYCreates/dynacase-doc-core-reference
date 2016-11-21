# SearchDoc::addFilter() {#core-ref:ec525c92-ab30-4861-aba1-7c2678df130a}

<div class="short-description">
La méthode addFilter permet d'ajouter un filtre sur les documents recherchés pour
ne trouver qu'un sous-ensemble des documents.
</div>

## Description {#core-ref:8b8dff10-b185-4a6c-80f4-25e937d9143d}

    [php]
    void addFilter(           string $filter, 
                   string|int|double $…)

Cette méthode permet de configurer la requête SQL qui va être générée. Elle
prend en argument un fragment de SQL et les associe pour créer la requête.

    [php]
    $searchDoc = new SearchDoc("", "IUSER");
    $searchDoc->addFilter("title = '%s'", "my user");
    $searchDoc->addFilter("id = %d", 12);

Les conditions SQL sont concaténés avec l'opérateur `AND`, par exemple la
requête  ci-dessus cherche les documents de type `IUSER` dont la propriété
`title` est  strictement égale à `my user` **et** la propriété `id` est égale à
12.

### Avertissements {#core-ref:9a5b484e-b2b2-4a48-8b78-75714b962661}

Les variables sont encodées avec la fonction [`pg_espace_string`][pgEscape] afin
de construire correctement les filtres et afin d'éviter l'injection SQL.

Si les variables contiennent du texte, celui-ci doit être encodé en
[UTF-8][utf8].

Cette méthode n'est pas utilisable conjointement avec une
[recherche spécialisée][core-ref:ssearch] en tant que
[collection de base][core-ref:SearchDoc::useCollection].

## Liste des paramètres {#core-ref:8596430f-cc20-46d7-9a9f-025dd48b0c51}

(string) `filter`
:   Partie filtre de la recherche, cette partie doit être soit une chaîne de 
    caractères contenant du SQL, soit une [chaîne formatée][phpSprintf] celle-ci
    est alors complétée avec les valeurs passées dans les arguments suivants.
    **Attention** : Les points suivants sont à prendre en compte :
    
    *   [injections SQL][wikipediaInjectionSQL] : le premier argument de la méthode
        addFilter est ajouté tel quel dans la requête SQL, il est donc possible qu'il 
        puisse déclencher une injection SQL. Les données doivent être 
        échappées (par exemple à l'aide de [pg_escape_string][pgEscape]),
    *   gestion de la multiplicité : les valeurs des attributs [multiples][multiple] (premier et second 
        niveau de multiplicité) sont stockées avec 
        [des caractères d'échappement][separateurMultiplicite]. La création d'une requête les prenant en
        compte nécessite l'utilisation d'opérateurs propre à postgreSql, il
        existe plusieurs manières d'aborder le problème :
        *   recherche à base [d'expressions régulières][postgresREGEXP] : `"ATTRNAME ~ E'\\\\y%s\\\\y'"` : il faut remplacer ATTRNAME par le nom de l'attribut.  
            **Limitation** : l'opérateur utilisé est une expression régulière avec le 
            séparateur de mot `\y` cela peut engendrer des faux positifs si jamais la 
            valeur contient elle-même des séparateurs de mot.  
            Ce type de recherche permet de faire les filtres les plus simples 
            (un parmi, etc.) avec le risque d'avoir des faux positifs.
        *   recherche à base de conversion en [array postgres][postgresARRAY], deux cas sont à prendre en compte :
            *   l'attribut a 1 niveau de multiplicité : `(regexp_split_to_array(ATTRNAME, E\'\\n\' ))`
            *   l'attribut a 2 niveaux de multiplicité : `(regexp_split_to_array(replace(ATTRNAME, \'<BR>\', E\'\\n\'), E\'\\n\' ))` 
            
            Dans les deux cas, il faut remplacer `ATTRNAME` par le nom de
            l'attribut.  Le comparaison se fait ensuite à l'aide des [opérateurs
            propres aux tableaux][postgresArrayOperator].
            
            Ce type de recherche est plus coûteux en ressource que le premier
            type mais permet de faire des recherches plus complexes (a des
            éléments en commun, est contenu par, etc.) et limite le risque de
            faux positif.

(string|int|double) `value`
:   Valeurs qui sont concaténées à la partie `filter` à l'aide la fonction 
    [`sprintf`][phpSprintf].  
    **Note** : Cet argument peut-être répété autant de fois
    que souhaité.  
    **Attention** : Les paramètres passés par `value` sont échappés à l'aide de la
    fonction [pg_escape_string][pgEscape].  
    **Attention** : Dans le cas de l'utilisation d'un opérateur à base d'expression
    régulière, il faut utiliser la fonction [preg_quote][preg_quote] sans quoi 
    les valeurs peuvent rendre l'expression régulière invalide.

## Valeur de retour {#core-ref:4a9e745b-c268-44c1-a1cb-6d05040083fd}

void

## Erreurs / Exceptions {#core-ref:c0b1b27b-4c43-45b9-9126-c5f8a03ac9ce}

Aucune.

## Historique {#core-ref:dd9da86c-ae52-4ff8-89ac-d9eebf8222cf}

Aucun.

## Exemples {#core-ref:13efc3fd-5ba6-4c96-a424-fe5d8cb9d771}

Recherche de documents dont l'attribut `firstname` est égal à "George" et
l'attribut `lastname` à "Abitbol" :

    [php]
    $searchDoc = new SearchDoc("", "IUSER");
    $searchDoc->addFilter("firstname = '%s' AND lastname = '%s'", "George", "Abitbol");

Recherche de documents dont l'attribut `firstname` est égal à "Arthur" et le
`lastname` à "O'Connor" :

    [php]
    $searchDoc = new SearchDoc("", "IUSER");
    $searchDoc->addFilter("firstname = '%s'", "Arthur");
    $searchDoc->addFilter("lastname = '%s'", "O'Connor");

Recherche de documents dont la propriété `titre` commence par "La classe"

    [php]
    $searchDoc = new SearchDoc("", "FILM");
    $searchDoc->addFilter("title ~* '^%s'", preg_quote("La classe"));

*Note* : On utilise ici l'opérateur [`~*`][postgresREGEXP] de postgresql.

Recherche de documents dont une des valeurs de l'attribut multivalué `actors`
contient "John Wayne" :

    [php]
    $searchDoc = new SearchDoc("", "FILM");
    $searchDoc->addFilter("actors ~* E'\\\\y%s\\\\y'", preg_quote("John Wayne"));

**Attention** : L'expression ci-dessus peut engendrer des faux positifs, par
exemple, si acteur contient `John Wayne Junior` il est aussi trouvé car `\y`
désigne un séparateur de mot et espace est un séparateur de mot.

Recherche de documents dont l'attribut multivalué actors contient `John Wayne`
et `Paul Newnam` :

    [php]
    $searchDoc = new SearchDoc("", "FILM");
    $searchDoc->addFilter("actors ~* E'\\\\y%s\\\\y'", preg_quote("John Wayne"));
    $searchDoc->addFilter("actors ~* E'\\\\y%s\\\\y'", preg_quote("Paul Newnam"));

**Attention** : L'expression ci-dessus peut engendrer des faux positifs.

Recherche de documents dont l'attribut multivalué actors contient `John Wayne`
et `Paul Newnam` :

    [php]
    $searchDoc = new SearchDoc("", "FILM");
    $searchDoc->addFilter("ARRAY[$s] && (regexp_split_to_array(replace(actors, \'<BR>\', E\'\\n\'), E\'\\n\' ))", "'Paul Newnam', 'John Wayne'");


## Notes {#core-ref:9c0906a3-9ee9-461e-ad85-f8294f4799d6}

Aucunes.

## Voir aussi {#core-ref:36e05af7-bfd1-480a-a643-339da40c3e69}

*   [`SearchDoc::addGeneralFilter()][addgeneralfilter]

<!-- links -->

[addgeneralfilter]:                     #core-ref:453cff11-09d9-4607-ab81-7acd36e99750
[core-ref:SearchDoc::useCollection]:    #core-ref:881c9fcb-81c2-45af-b89f-70be3a7f24b7
[core-ref:ssearch]:                     #core-ref:1eb7c0ad-dabd-47e9-83bf-f9e3d4eca806
[multiple]:                             #core-ref:324c7c7e-bd80-4c19-ad24-daf0f39caa61
[pgEscape]:                             http://us1.php.net/manual/en/function.pg-escape-string.php "pg_escape_string"
[phpSprintf]:                           http://us3.php.net/manual/en/function.sprintf.php "PHP : sprintf"
[postgresARRAY]:                        http://www.postgresql.org/docs/9.1/static/functions-array.html "Postgres : Array"
[postgresArrayOperator]:                http://www.postgresql.org/docs/9.1/static/functions-comparisons.html "Postgres : Array operators"
[postgresREGEXP]:                       http://www.postgresql.org/docs/9.1/static/functions-matching.html#FUNCTIONS-POSIX-REGEXP "Postgres : POSIX Regular Expressions"
[preg_quote]:                           http://us2.php.net/preg_quote "PHP : preg_quote"
[separateurMultiplicite]:               #core-ref:1b8cd020-a2ed-4997-aefe-a4fcbb3564f1
[utf8]:                                 http://fr.wikipedia.org/wiki/UTF-8 "UTF-8 sur Wikipédia"
[wikipediaInjectionSQL]:                https://fr.wikipedia.org/wiki/Injection_SQL "Injection SQL"
