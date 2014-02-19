# Définition de valeurs d'énumérés {#core-ref:eef3e3ec-2d50-41bd-98e1-cc978f0a5178}

Les énumérés sont structurés sous la forme de couples *clé*/*valeur*. La valeur
stockée en base de données est la clé, alors que l'utilisateur voit la valeur
correspondante.

Les labels peuvent ainsi être traduits, tout en n'altérant pas le stockage en
base.

Lors de la [Déclaration de familles][declaration_famille], il est possible de
définir des couples de *clé*/*valeur* au moyen de la syntaxe
`[cle1]|[label 1],[cle2]|[label 2],…`.

Pour chaque clé définie par ce moyen :

*   si la clé existe déjà pour cet énuméré, le label correspondant est mis à
    jour,
*   si la clé n'existe pas pour cet énuméré, elle est ajoutée.

## Restrictions sur la valeur des clés {#core-ref:bd7ca69d-80a6-4f44-a2c3-fcf83a0abd69}

Lors de la composition des clés, il est interdit d'utiliser

*   les caractères :
    *   `"` (*guillemet double*),
    *   `'` (*apostrophe*),
    *   `&` (*esperluette*),
    *   `|` (*pipe*)
*   les séquences :
    *   `-dot-`,
    *   `-comma-`

En outre, les valeurs suivantes doivent être échappées au moyen du caractère
`\` (*anti-slash*) :

*   `.` (*point*) : voir les [énumérés multi-niveaux][multi_level]

## Restrictions sur la valeur des labels {#core-ref:8c21b7c1-91e7-445c-ab0b-b149e77eaf97}

Lors de la composition des labels, il est interdit d'utiliser

*   les caractères :
    *   `|` (*pipe*)
*   les séquences :
    *   `-dot-`,
    *   `-comma-`

En outre, les valeurs suivantes doivent être échappées au moyen du caractère
`\` (*anti-slash*) :

*   `,` (*virgule*)

## Suppression de valeurs d'énumérés {#core-ref:05f987d2-3901-4764-9bf8-96c104ec7bf5}

Si l'énuméré est déclaré avec l'option `system=yes`, les définitions des
énumérés est réinitialisé à chaque importation.

Si l'énuméré n'a pas cette option, il faut ajouter un ordre
[`RESET;enums`][resetfamille] pour forcer la réinitialisation avec les nouvelles
valeurs définies.

**Note** : La suppression d'une valeur dans la définition ne supprime pas les
valeurs déjà affectés aux documents. Si un document a une valeur non répertoriée
dans la définition de l'énuméré ce sera cette valeur (brute) qui sera affichée
car son ancien libellé aura été supprimé.

## Énumérés multi-niveaux {#core-ref:fd01b48a-6da5-4eef-90e8-d2d274a9e9ce}

Il est possible d'exprimer une *arborescence* des clés au moyen du séparateur
`.`.

Par exemple, l'énuméré
`france|France,france.midi|Midi-Pyrénées,france.midi.gers|Gers,france.midi.haute-garonne|Haute-Garonne,france.idf|Île-de-France,france.idf.paris|Paris`
définit l'arborescence :

*   France
    *   Midi-Pyrénées
        *   Gers
        *   Haute-Garonne
    *   Île-de-France
        *   Paris

Du point de vue interne, c'est la clé 'finale' qui est stockée. Cela permet de
modifier l'arborescence sans impacter les clés déjà stockées. Cependant, cela
implique aussi que **toutes les clés finales d'un énuméré doivent être
non‑ambigües** (par exemple, `un|un,un.one|one,deux|deux,deux.one|one more`
n'est pas valide, car la clé `one` désigne à la fois `un.one` et `deux.one`).

## Valeurs dynamiques {#core-ref:df6f9457-51d9-4682-89ac-8825f2719779}

La définition d'un énuméré peut être fournie par une fonction PHP.

Cette fonction est alors exprimée de la même façon qu'une aide à la saisie : la
caractéristique `phpfile` doit désigner un fichier publié dans le répertoire
`EXTERNALS` et la fonction de définition de l'énuméré doit être définie dans le
fichier référencé par `phpfile`.

Cette fonction doit retourner la définition sous la forme d'une chaîne de
caractères comme décrit ci-dessus.

## Localisation des énumérés {#core-ref:b7e4f6cc-adce-4fc2-bd8b-dada5abb3477}

Les labels sont traduisibles. les clés de traductions sont composées sous la
forme `<FAMILYNAME>#<ATTRID>#<ENUMKEY>` (exemple `MY_FAMILY#my_color#yellow`).

<!-- links -->
[declaration_famille]: #core-ref:cfc7f53b-7982-431e-a04b-7b54eddf4a75
[multi_level]: #core-ref:fd01b48a-6da5-4eef-90e8-d2d274a9e9ce
[resetfamille]: #core-ref:5c661733-772d-42b8-8b3e-b70453ddfd33
