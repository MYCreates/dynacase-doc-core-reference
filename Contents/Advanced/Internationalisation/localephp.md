# Utiliser une traduction dans un programme PHP {#core-ref:967cd878-e068-4c99-8266-adaed3f700ff}

## Utiliser une forme simple {#core-ref:3275febc-4171-11e3-9773-cffb8e583c34}

La fonction fournie en standard par PHP et la fonction [gettext][phpGettext].
Cette fonction permet de rechercher dans un catalogue la traduction
correspondante au texte donné en paramètre.

    [php]
    // texte simple
    print gettext("This is a text");
    // texte avec une partie variable
    printf(gettext("An error : [%s]"), $errorText);

Note : La fonction `_` de PHP est un alias de `gettext`.

    [php]
    // écriture plus lisible en utilisant l'alias.
    print _("This is a text");
    printf(_("An error : [%s]"), $errorText);


**Attention** : La fonction `gettext` doit avoir comme argument un texte non vide.
Si le texte est une chaîne vide alors c'est la description du catalogue qui est
retournée.

Le texte à traduire est généralement écrit en anglais (ASCII) et ne comporte pas de
caractère accentué. Ceci permet d'éviter les problème d'encodage lors de la
génération du catalogue. Il est cependant possible de mettre des textes à
traduire avec des accents (voir [chapitre suivant][gencatalog]).

Dans l'exemple précédent, le texte est très commun et il peut être défini dans
un autre module avec une traduction différente. 

Deux possibilités permettent de diminuer ce risque de doublon :

1.  Utiliser un préfixe :
    
        [php]
        print _("MyCatalog:This is a text");

2.  Utiliser un contexte : <span class="flag from release">3.2.12</span>
    
        [php]
        print ___("This is a text", "MyContext");

La différence entre le préfixe et le [contexte][pgettext] est que le contexte est
explicitement indiqué dans le catalogue tandis que le préfixe est pris comme un
texte simple.

La fonction `___` (triple blancs soulignés) est un alias de la fonction
`pgettext`. Si le contexte est vide, cela est équivalent à l'utilisation de la
fonction `_` standard.

Ces fonctions ne sont pas natives de PHP, elles sont ajoutées par Dynacase.

## Utiliser une forme plurielle {#core-ref:3e6b8eee-4171-11e3-9688-cffb8e583c34}

<span class="flag from release">3.2.12</span>
Les traductions des formes plurielles sont prises en compte par la bibliothèque
_gettext_.

La forme plurielle est indiquée à l'aide la fonction php standard
[ngettext][ngettext] ou de la fonction `npgettext` et son alias `n___` qui
gèrent aussi les contextes.

    [php]
    printf(n___("%d document found", "%d documents found", $num, "MyContext") , $num);

| langue | $num |    texte traduit      |   forme   |
| ------ | ---- | --------------------- | --------- |
| fr     |    0 | 0 document trouvé     | singulier |
| fr     |    1 | 1 document trouvé     | singulier |
| fr     |    2 | 2 documents trouvés   | pluriel   |
| fr     |  345 | 345 documents trouvés | pluriel   |
|        |      |                       |           |
| en     |    0 | 0 documents found     | pluriel   |
| en     |    1 | 1 document found      | singulier |
| en     |    2 | 2 documents found     | pluriel   |
| en     |  345 | 345 documents founds  | pluriel   |

*Limitations* : La localisation des formes plurielles fonctionne de manière
partielle avec les nombres décimaux. En effet, la variable numérique de choix de
la traduction doit être un entier. Pour un nombre décimal, seule la partie
entière est prise en compte. Ainsi les nombres _1,3_ et _1,6_ seront pris comme
_1_. Cela sera correct pour le français qui considère que tout nombre compris
entre _]-2, 2[_ comme singulier. Mais cela sera incorrect en anglais qui
considère que tout nombre différent de 1 ou -1 n'est pas singulier.

<!-- link -->
[wikiGettext]:       http://fr.wikipedia.org/wiki/GNU_gettext "Gettext sur Wikipédia"
[phpGettext]:        http://www.php.net/manual/fr/function.gettext.php "gettext sur php.net"
[actions]:           #core-ref:e67d8aeb-939c-46e3-9be8-6fc3ba75ebc2 "Action Dynacase"
[wsh]:               #core-ref:4df1314f-9fdd-4a7f-af37-a18cc39f3505 "Script Dynacase"
[gencatalog]:        #core-ref:2c163f00-8e94-4736-86f2-bb51352c52aa
[pgettext]:          http://www.gnu.org/software/gettext/manual/html_node/Contexts.html "Contexte dans gettext"
[ngettext]:          http://www.php.net/manual/fr/function.ngettext.php "ngettext sur php.net"
[layout]:           #core-ref:5f4a2f4b-9ceb-42db-8ac1-2a7baa621ce2
[xgettext]:         http://www.gnu.org/software/gettext/manual/html_node/xgettext-Invocation.htm "xgettext reference"
[famdecl]:          #core-ref:cfc7f53b-7982-431e-a04b-7b54eddf4a75
[gettextutil]:      http://www.gnu.org/software/gettext/manual/html_node/index.html#Top