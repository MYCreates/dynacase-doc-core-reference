# Type `array` {#core-ref:dd400581-8896-4eec-9b9e-f1e5669cf180}

## Description {#core-ref:324c7c7e-bd80-4c19-ad24-daf0f39caa61}

Les attributs de type *array* permettent de définir des tableaux.

Chacun des attributs contenu dans un *array* est alors dit *multiple*, et correspond à une colonne du dit *array*. Un tableau ne peut pas contenir d'autres attributs de type `array`.

## Représentation {#core-ref:818bdf5f-c037-41b6-9c64-6e2a23a685d4}

Les array sont représentés au moyen d'une `table`.

Chaque colonne correspond à un attribut contenu dans l'array.
Le libellé de la colonne est le libellé de l'attribut correspondant.

*   consultation :
    
    Un tableau.
    
    ![ array - consultation html ](famille/attributs/array-consultation.png "array - Consultation html")

*   modification :
    
    La première colonne contient les outils permettant la sélection, suppression, et le déplacement d'une colonne,
    alors que la dernière ligne contient les outils permettant de rajouter une ligne ou de dupliquer une ligne existante.
    
    ![ array - Modification html ](famille/attributs/array-modification.png "array - Modification html")

*   odt :
    
    Un tableau
    
    ![ array - consultation odt ](famille/attributs/array-odt.png "array - Consultation odt")

## Comportement {#core-ref:411d067e-4729-43aa-a33b-49915e131396}

Aucun comportement particulier.

## Format de stockage {#core-ref:2448fbfc-c138-4134-aa46-800080f63738}

Le tableau en lui même n'est pas stocké, mais modifie la façon dont les attributs qu'il contient sont stockés.
En effet, une fois dans un tableau, un attribut est multiple, et nécessite donc le stockage de plusieurs valeurs.
Pour rester simple, la valeur stockée sera une suite des valeurs de stockage simples séparées par un séparateur interne.

Pour plus de précisions, se reporter au chapitre avancé sur les formats de stockage.

## Options applicables à l'array {#core-ref:9eb6f53f-158d-497d-a472-2602a195cbce}

cellbodystyle
:   Indique le style css appliqué sur les cellules du corps de tableau.
    
    Les valeurs possibles sont :
    
    *   toute définition css valide.

cellheadstyle
:   Indique le style css appliqué sur les cellules de l'entête de tableau.
    
    Les valeurs possibles sont :
    
    *   toute définition css valide.

classname
:   Indique une classe css à appliquer aux cellules du corps tableau en consultation.
    
    Les valeurs possibles sont :
    
    *   tout nom de classe valide.

displayrowcount <span class="flag from release">3.2.17</span>
:   Indique si le nombre de lignes du tableau est affiché dans l'entête de la première colonne.
    
    Les valeurs possibles sont un entier _n_ avec :
    
    * _n_ <= -1 pour ne jamais afficher le nombre de lignes ;
    * _n_ = 0 pour toujours afficher le nombre de lignes ;
    * _n_ > 0 pour afficher le nombre de lignes si celui-ci est > _n_.
    
    Par défaut la valeur de `displayrowcount` est `10`.

empty
:   Indique que le tableau, s'il est vide ne doit pas afficher la première rangée en modification.
    Dans le cas contraire, en modification, le tableau est initialisé avec une rangée vide.
    
    Les valeurs possibles sont :
    
    *   `yes`
    *   **`no` (comportement par défaut)**

height
:   Indique la hauteur du corps du tableau.
    Si le corps du tableau dépasse la hauteur spécifiée, un ascenseur vertical apparaîtra.
    
    Ne fonctionne qu'avec les navigateurs récents.
    
    Les valeurs possibles sont:
    
    *   Une taille en pixels (par exemple `150px`).

sorttable
:   Indique que le tableau est triable.
    L'utilisateur peut cliquer sur un en-tête de colonne pour trier cette colonne.
    
    Le tri est effectué au moyen du script [sorttable.js][SORTTABLEJS_home].
    
    L'utilisation avancée de sorttable.js sort du sujet de cette documentation, et est à charge et de la responsabilité du développeur.
    
    Les valeurs possibles sont :
    
    *   `yes`
    *   **`no` (comportement par défaut)**

twidth
:   Indique la largeur du tableau.
    Si le corps du tableau dépasse la largeur spécifiée, un ascenseur horizontal apparaîtra.
    
    Les valeurs possibles sont :
    
    *   une valeur absolue en pixel (par exemple `100px`),
    *   une largeur relative en pourcentage (par exemple `30%`)
    
    La valeur par défaut est `100%`.

userowadd
:   Indique si l'utilisateur est autorisé à ajouter des rangées au tableau.
    Dans ce cas, le bouton correspondant est affiché.
    
    Cela permet de ne pas être en conflit si le tableau doit être rempli par un code spécifique sur l'interface, mais pas par l'utilisateur.
    
    Les valeurs possibles sont :
    
    *   **`yes` (comportement par défaut)**
    *   `no`

rowviewzone
:   Indique une [vue spécifique de tableau][viewrow] pour la consultation.

roweditzone
:   Indique une [vue spécifique de tableau][editrow] pour la modification.

## Options applicables aux attributs contenus dans un array {#core-ref:2732ecb2-7cc1-414f-8864-7d7e97a6091b}

align
:   Indique l'alignement horizontal pour les cellules de la colonne.
    
    Les valeur possibles sont :
    
    *   `left`,
    *   `right`,
    *   `center`,
    *   `justify`

bgcolor
:   Indique la couleur de fond des cellules de la colonne.
    
    Les valeurs possibles sont :
    
    *   toute [couleur css valide][MDN_css_color_value]
        (exemple : `yellow`, `#FF335A`, etc.)

color
:   Indique la couleur du texte pour les cellules de la colonne.
    
    Les valeurs possibles sont :
    
    *   toute [couleur css valide][MDN_css_color_value]
        (exemple : `yellow`, `#FF335A`, etc.)

cwidth
:   Indique la largeur de la colonne.
    
    Les valeurs possibles sont :
    
    *   une valeur absolue en pixel (par exemple `100px`),
    *   une largeur relative en pourcentage (par exemple `30%`)
    *   **`auto` (comportement par défaut)**

<!-- links -->
[MDN_css_color_value]: https://developer.mozilla.org/en-US/docs/CSS/color_value "description du type css color sur MDN"
[MDN_css_length_value]: https://developer.mozilla.org/en-US/docs/CSS/length "description du type css length sur MDN"
[PHP_money_format]: http://php.net/manual/fr/function.money-format.php "documentation de money_format sur php.net"
[PHP_is_numeric]: php.net/manual/function.is-numeric.php "documentation sur php.net"
[CKEDITOR_home]: http://ckeditor.com/ "Site officiel de CKEditor"
[CKEDITOR_option]: http://docs.cksource.com/ckeditor_api/symbols/CKEDITOR.config.html "options de CKEDITOR"
[SORTTABLEJS_home]: http://www.kryogenix.org/code/browser/sorttable/ "site officiel de sorttable.js"
[JSCOLOR_home]: http://jscolor.com/ "site officiel de JSColor"
[JSCALENDAR_HOME]: http://www.dynarch.com/projects/calendar/old/ "site officiel de JSCalendar"
[odt_restrictions]: #core-ref:3742b35d-ddc0-440e-a0aa-08ea2faf0e46
[viewrow]:  #core-ref:d237e0d4-ca4e-4b9e-8249-45aef7d2ef0f
[editrow]:  #core-ref:c0ed29f6-02e5-4ff2-8943-511a7d00c106
