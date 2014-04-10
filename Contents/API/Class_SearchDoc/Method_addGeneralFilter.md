# SearchDoc::addGeneralFilter() {#core-ref:453cff11-09d9-4607-ab81-7acd36e99750}

<div class="short-description">
Cette fonction permet de faire une <a
href="https://en.wikipedia.org/wiki/Full_text" title="Wikipedia : full text
search">recherche full text</a> sur un document.
</div>

## Description {#core-ref:9a461deb-2fa3-4c02-939b-c1d5425e77c1}

    [php]
    void addGeneralFilter ( string $filter, 
                              bool $useSpell )

### Avertissements {#core-ref:b951b810-4d27-433c-8fee-6a9d7f99dee3}

Cette recherche ne fonctionne que en langue française. Pour les autres langues
notamment la langue anglaise, la recherche peut donner des résultats
inappropriés. Ceci est du au fait que la [lemmatisation][lemmatisation], source
de l'indexation, est réalisée pour le français .

## Liste des paramètres {#core-ref:f45c00e1-0b07-442f-b321-f8e7b34b85ea}

(string) `filter`
:   Cette chaîne de caractères contient le filtre dans le format de la 
    [recherche globale][globalSearch].

(bool) `useSpell`
:   Indique si une correction orthographique sur les mots de la langue française
    doit être appliquée avant la recherche.  
    Dans ce cas, si le mot n'est pas trouvé alors le filtre retourne le mot et 
    le mot [le plus approchant trouvé][aspell].

## Valeur de retour {#core-ref:5b4d4af5-b668-4d20-861e-cf749e520e58}

void

## Erreurs / Exceptions {#core-ref:68594dbd-ad1d-4e86-bfb3-071bf82d5c09}

<span class="flag from release">3.2.12</span>
Si le filtre indiqué est invalide une exception `Dcp\SearchDoc\Exception` est
envoyée.

## Historique {#core-ref:74230984-e371-4766-8b0e-15152448f1db}

### Release 3.2.12 {#core-ref:b70cbb23-de63-4132-8334-20a80ccc7c76}

Si le filtre indiqué est invalide une exception est retournée que soit l'erreur.
Auparavant, une exception était retournée que lorsque la vérification de premier
niveau détectait l'erreur.

Le type de l'exception est passé de `\Dcp\Exception\` à
`Dcp\SearchDoc\Exception`.

## Exemples {#core-ref:33fa9341-47a0-400d-8664-698efdf9aadf}

Recherche de tous les documents contenant les trois mots _"crêpes aux
fromages"_. Dans ce cas précis le mot _"aux"_ n'est pas pris en compté car il
fait partie [des mots non significatifs][stopword].

    [php]
    $s=new SearchDoc('',"COOKBOOK");
    $s->addGeneralFilter('crêpes aux fromages'); 
    $s->search();

Recherche de tous les documents contenant le mot "crêpe" et soit "champignon" ou 
soit "fromage".

    [php]
    $s=new SearchDoc('',"COOKBOOK");
    $s->addGeneralFilter('crêpes (champignons OR fromage)'); 
    $s->search();

## Notes {#core-ref:bad0fd72-37c8-4bf9-9b57-a8716ed6b434}

Aucunes.

## Voir aussi {#core-ref:94b287b2-3cc0-4d9d-8102-280a94aeb64f}


Voir la [recherche globale][globalSearch].

*   [`addFilter()`][addFilter]

<!-- links -->
[globalSearch]:     #core-ref:19b9f4b4-c960-46eb-b4e0-805ed76be3a6
[lemmatisation]:    http://fr.wikipedia.org/wiki/Lemmatisation "Définition sur Wikipédia"
[aspell]:           http://fr.wikipedia.org/wiki/GNU_Aspell "Aspell sur Wikipédia"
[addFilter]:        #core-ref:ec525c92-ab30-4861-aba1-7c2678df130a
[stopword]:         http://fr.wikipedia.org/wiki/Mot_vide "Stop Words : définition sur Wikipédia"