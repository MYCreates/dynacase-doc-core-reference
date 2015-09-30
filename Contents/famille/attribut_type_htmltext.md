# Type `htmltext` {#core-ref:6ebf113a-1d6a-4c7a-be51-955955056bb3}

## Description {#core-ref:97b8259b-4df3-4450-99b0-c7d8fb5db785}

Les attributs de type *htmltext* permettent d'insérer du texte avec mise en forme.

Le langage de mise en forme est le html, et un éditeur WYSIWYG (basé sur [CKEditor][CKEDITOR_home]) permet d'en simplifier la saisie.

## Représentation {#core-ref:8f0bfff9-aaa6-4767-b905-d67a954e2ffa}

*   consultation :
    
    une `div` contenant la valeur.
    
    ![ htmltext - consultation html ](famille/attributs/htmltext-consultation.png "htmltext - Consultation html")

*   modification :
    
    une `iframe` dans laquelle est initialisé une instance de CKEditor.
    
    ![ htmltext - Modification html ](famille/attributs/htmltext-modification.png "htmltext - Modification html")

*   odt :
    
    la mise en forme est conservée.
    
    Se reporter au chapitre sur les vues odt
    pour les [restrictions][odt_restrictions] sur l'utilisation des attributs de
    type html dans les document openDocument text.

## Comportement {#core-ref:9bad041e-d747-4d9b-9406-5b62d662229b}

L'éditeur CKeditor nettoit le code HTML inséré. Il ne conserve que les balises
qui peuvent être insérées avec la barre de menu.

<span class="flag from release">3.2.19</span> Lors de l'enregistrement, 
le nettoyage suivant est effectué :

*   Suppression de tous les attributs commençant par "on" ou "xmlns"
*   Suppression du javascript, applet et vbscript (le contenu est conservé sans les balises)
*   Suppression des éléments avec namespace
*   Les entités UTF-8 sont décodées ;
*   Les commentaires sont supprimés.

## Format de stockage {#core-ref:083d2816-0682-4c35-b248-caa6e162b80e}

La valeur stockée est le html *nettoyé* selon les règles précédentes.

Le type utilisé en base de donnée est `text`. 

## Options {#core-ref:8e182116-8762-4157-a743-9abf43db0960}

En plus des [options communes à tous les types d'attributs](#core-ref:16e19c90-3233-11e2-a58f-6b135c3a2496), ce type d'attribut dispose des options suivantes :

doclink
:   Cette option nécessite l'installation du module dynacase-ckeditor-plugins.
    
    Active l'option doclink du HTMLEditor. Cette option se présente sous la forme d'un nouveau bouton (dans la partie réservée au lien dans les toolbar et en fin de toolbar sur la Basic), en cliquant sur le bouton une interface vous propose de sélectionner un document et ajoute une balise de lien vers ce document.
    
    Les valeurs possibles sont :
    
    *   un objet JSON (par exemple `{"famId": "DIR", "docrev" : "fixed"}`), contenant les propriétés suivantes :
        *   `famId` (obligatoire) : nom logique de la famille des documents cible,
        *   `docrev` (facultatif) : les valeurs possibles sont les mêmes que l'option [*docrev*][docrev] des attributs de type *docid* (*latest*, *fixed*, *state(keystate)*),
        *   `filter` (facultatif) : un filtre SQL qui sera appliqué à la recherche

editheight
:   Indique la hauteur de la zone d'édition.
    
    Les valeurs possibles sont :
    
    *   une taille en pixels (par exemple `400px`)
    
    La valeur par défaut est `150px`.

htmlclean
:   Indique que le serveur nettoiera le contenu en supprimant toutes les balises 
    de style, généralement issues d'un copier/coller :
    
    *   les attribut des balises `span` (le contenu est conservé) et `font` sont supprimés ;
    *   les attributs `@class` et `@style` sont supprimés ;
    *   les balises `style` sont supprimées.
    
    Les valeurs possibles sont :
    
    *   `yes`
    *   **`no` (comportement par défaut)**

allowedcontent <span class="flag from release inline">3.2.20</span>
:   Indique que la liste des balises autorisées n'est pas liée au menu. Toute balise
    html supportée peut être insérée à l'exception de la balise `script`. 
    Les attributs de ces balises peuvent aussi 
    être utilisées à l'exception des attributs commençant par `on` tel que 
    `onclick` ou `onmouseover`.
    
    Les valeurs possibles sont :
    
    *   `all` : toute balise supportée est autorisée
    *   **`menu` (comportement par défaut)** en fonction de la barre de menu (voir option `toolbar` ci-après).
    
    Les balises supportées sont : `a`, `abbr`, `acronym`, `address`, `applet`,
    `area`, `article`, `aside`, `audio`, `b`, `base`, `basefont`, `bdi`, `bdo`,
    `big`, `blockquote`, `body`, `br`, `button`, `canvas`, `caption`, `center`,
    `cite`, `code`, `col`, `colgroup`, `command`, `datalist`, `dd`, `del`,
    `details`, `dfn`, `dialog`, `dir`, `div`, `dl`, `dt`, `em`, `embed`,
    `fieldset`, `figcaption`, `figure`, `font`, `footer`, `form`, `h1`, `h2`,
    `h3`, `h4`, `h5`, `h6`, `head`, `header`, `hgroup`, `hr`, `html`, `i`,
    `iframe`, `img`, `input`, `ins`, `isindex`, `kbd`, `keygen`, `label`,
    `legend`, `li`, `link`, `main`, `map`, `mark`, `menu`, `meta`, `meter`,
    `nav`, `noframes`, `noscript`, `object`, `ol`, `optgroup`, `option`,
    `output`, `p`, `param`, `pre`, `progress`, `q`, `rp`, `rt`, `ruby`, `s`,
    `samp`, `section`, `select`, `small`, `source`, `span`, `strike`,
    `strong`, `style`, `sub`, `summary`, `sup`, `table`, `tbody`, `td`,
    `textarea`, `tfoot`, `th`, `thead`, `time`, `title`, `tr`, `track`, `tt`,
    `u`, `ul`, `var`, `video`, `wbr`.


jsonconf
:   Cette option permet de configurer finement l'éditeur de texte WYSIWYG.
    
    Elle permet notamment de fixer finement le comportement de l'éditeur et le contenu des toolbar.
    Les options de configuration sont [celles de CKEditor][CKEDITOR_option], et l'objet de configuration doit être présenté en JSON valide.
    
    Les options propres à CKEditor (*resize*, *correction orthographique*, etc) ne sont pas maintenues par Anakeen
    et leur bon fonctionnement n'est pas garanti par Anakeen.
    
    Aux options de CKEDITOR, dynacase ajoute les options suivantes :
    
    *   addPlugin : permet de charger un plugin additionnel.
        
        La valeur doit être un tableau (JSON) de noms logiques de plugins (par exemple: `"addPlugins": ["docattr"]`).
        Dans ce cas, le plugin doit posséder une commande ayant le même nom que le nom logique du plugin.
        Cette commande est alors ajoutée en fin de toolbar.
    
    Exemple de configuration permettant d'activer le plugin docattr et le plugin doclink et avec un menu basique et l'activation du mode resize :
    `jsonconf={"addPlugins" : ["docattr"], "doclink" :   {"famId" : "DIR"}, "toolbar" : "basic", "resize_enabled" : true}`
    
    **Attention** : l'utilisation de cette option bas niveau désactive les options suivantes :
    
    *   toolbar
    *   toolbarexpand
    *   editheight
    *   doclink
    
    Il est par contre possible de construire une option *jsonconf* qui a le même effet que les options précédentes.


toolbar
:   Indique le template à utiliser pour la barre de menu.
    
    
    La barre de menu contraint par défaut la liste des éléments acceptés par 
    l'éditeur de texte.
    
    Pour supprrimer cette contrainte, il faut utiliser l'option [`allowedContent`][CKEDITOR_option]
    de ckeditor.
    Exemple pour tout autoriser : `jsonconf={"allowedContent" : true, "toolbar" : "Basic"}`
    
    Les valeurs possibles sont :
    
    *   **`Simple` (comportement par défaut)** :
        
        ![toolbar Simple](famille/attributs/htmltext-toolbar-simple.png)
        
        Les balises et attributs autorisés sont : 
        * "a[_tout attribut_] "
        * "address"
        * "br"
        * "caption[text-align]"
        * "div[text-align]"
        * "em"
        * "h1[text-align]"
        * "h2[text-align]"
        * "h3[text-align]"
        * "h4[text-align]"
        * "h5[text-align]"
        * "h6[text-align]"
        * "img[_tout attribut_] "
        * "li[text-align]"
        * "ol"
        * "p[text-align]"
        * "pre[text-align]"
        * "s"
        * "span[font-size, color, background-color]"
        * "strong"
        * "table[width, height]"
        * "tbody"
        * "td[width, height, border-color, background-color, white-space, vertical-align, text-align, text-align]"
        * "tfoot"
        * "th[width, height, border-color, background-color, white-space, vertical-align, text-align, text-align]"
        * "thead"
        * "tr"
        * "u"
        * "ul"
    
    *   `Basic` :
        
        ![toolbar Basic](famille/attributs/htmltext-toolbar-basic.png)
      
        Les balises et attributs autorisés sont : 
        * "a[_tout attribut_] "
        * "br"
        * "em"
        * "img[_tout attribut_] "
        * "li"
        * "ol"
        * "p"
        * "strong"
        * "td[width, height, border-color, background-color, white-space, vertical-align, text-align]"
        * "th[width, height, border-color, background-color, white-space, vertical-align, text-align]"
        * "ul"
    
    *   `Default` :
        
        ![toolbar Default](famille/attributs/htmltext-toolbar-default.png)
        
        Les balises et attributs autorisés sont : 
        * "a[_tout attribut_] "
        * "address"
        * "big"
        * "blockquote"
        * "br"
        * "caption[text-align]"
        * "cite"
        * "code"
        * "del"
        * "div[text-align, padding, background, border]"
        * "em"
        * "h1[text-align]"
        * "h2[text-align, font-style]"
        * "h3[text-align, color, font-style]"
        * "h4[text-align]"
        * "h5[text-align]"
        * "h6[text-align]"
        * "hr"
        * "img[_tout attribut_] "
        * "ins"
        * "kbd"
        * "li[text-align]"
        * "ol"
        * "p[text-align]"
        * "pre[text-align]"
        * "q"
        * "s"
        * "samp"
        * "small"
        * "span[font-family, font-size, color, background-color]"
        * "strong"
        * "sub"
        * "sup"
        * "table[width, height, border-collapse, border-style, background-color]"
        * "tbody"
        * "td[width, height, border-color, background-color, white-space, vertical-align, text-align, text-align]"
        * "tfoot"
        * "th[width, height, border-color, background-color, white-space, vertical-align, text-align, text-align]"
        * "thead"
        * "tr"
        * "tt"
        * "u"
        * "ul[list-style-type]"
        * "var"
    
    *   `Full` :
        
        ![toolbar Full](famille/attributs/htmltext-toolbar-full.png)
        
        Les balises et attributs autorisés sont : 
        * "a[_tout attribut_] "
        * "address"
        * "big"
        * "blockquote"
        * "br"
        * "caption[text-align]"
        * "cite"
        * "code"
        * "del"
        * "div[text-align, padding, background, border]"
        * "em"
        * "h1[text-align]"
        * "h2[text-align, font-style]"
        * "h3[text-align, color, font-style]"
        * "h4[text-align]"
        * "h5[text-align]"
        * "h6[text-align]"
        * "hr"
        * "img[_tout attribut_] "
        * "ins"
        * "kbd"
        * "li[text-align]"
        * "ol"
        * "p[text-align]"
        * "pre[text-align]"
        * "q"
        * "s"
        * "samp"
        * "small"
        * "span[font-family, font-size, color, background-color]"
        * "strong"
        * "sub"
        * "sup"
        * "table[width, height, border-collapse, border-style, background-color]"
        * "tbody"
        * "td[width, height, border-color, background-color, white-space, vertical-align, text-align, text-align]"
        * "tfoot"
        * "th[width, height, border-color, background-color, white-space, vertical-align, text-align, text-align]"
        * "thead"
        * "tr"
        * "tt"
        * "u"
        * "ul[list-style-type]"
        * "var"

toolbarexpand
:   Indique si la barre de menu doit être dépliée lors de l'affichage de l'éditeur.
    Dans le cas contraire, un petit bouton permet de la déplier.
    
    Les valeurs possibles sont :
    
    *   **`yes` (comportement par défaut)**
    *   `no`


<!-- links -->
[MDN_css_color_value]: https://developer.mozilla.org/en-US/docs/CSS/color_value "description du type css color sur MDN"
[MDN_css_length_value]: https://developer.mozilla.org/en-US/docs/CSS/length "description du type css length sur MDN"
[PHP_money_format]: http://php.net/manual/fr/function.money-format.php "documentation de money_format sur php.net"
[PHP_is_numeric]: php.net/manual/function.is-numeric.php "documentation sur php.net"
[CKEDITOR_home]: http://ckeditor.com/ "Site officiel de CKEditor"
[CKEDITOR_option]: http://docs.ckeditor.com/#!/api/CKEDITOR.config "options de CKEDITOR"
[SORTTABLEJS_home]: http://www.kryogenix.org/code/browser/sorttable/ "site officiel de sorttable.js"
[JSCOLOR_home]: http://jscolor.com/ "site officiel de JSColor"
[JSCALENDAR_HOME]: http://www.dynarch.com/projects/calendar/old/ "site officiel de JSCalendar"
[odt_restrictions]: #core-ref:3742b35d-ddc0-440e-a0aa-08ea2faf0e46
[docrev]: #core-ref:9bcfd205-fb07-4a71-be06-ba07d4a9cc7c