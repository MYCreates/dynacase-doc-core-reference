# Gestion des styles {#core-ref:1844a1a8-1406-47bd-a884-1a18ef0a6ca7}
## Description des styles {#core-ref:34e4800a-48c9-47d3-9694-e720e0fa624a}

### Éléments de style {#core-ref:a5ec398a-58d4-4584-a071-e77299057c41}

Un style est composé de :

*   Un fichier de description, 
*   un fichier _css_ (optionnel)
*   des images (optionnel)

Soit le style `MONSTYLE`.
Les fichiers doivent être publiés sur le serveur avec cette arborescence :

*   STYLE
    *   MONSTYLE
        *   MONSTYLE.sty
        *   Layout
            *   MONSTYLE.css
        *   Images
            *   monimage1.png
            *   monimage2.png

Le fichier de style est `MONSTYLE.sty`, le fichier _css_ est `MONSTYLE.css`.

### Fichier de style {#core-ref:53fe66e1-ecb6-4f0b-b7b4-9d9a420ecbcf}

Un fichier de style `.sty` est un fichier php, déclarant les variables :

**`$sty_desc` (obligatoire)**
:   La description du style.
    
    C'est un tableau contenant les entrées :
    
    **`name` (obligatoire)**
    :   Le nom du style.
    
    `description`
    :   La description du style
    
    `parsable` *(déprécié)*
    :   conservé pour compatibilité avec l'ancien système.

**`$sty_colors` (obligatoire)**
:   Les couleurs de base du style.
    
    C'est un tableau contenant les entrées :
    
    *   **`A` (obligatoire)**
    *   **`B` (obligatoire)**
    *   **`C` (obligatoire)**
    
    Ces variables seront utilisées pour générer les paramètres de couleur
    (de `COLOR_A0` à `COLOR_A9`, de `COLOR_B0` à `COLOR_B9` 
    et de `COLOR_C0` à `COLOR_C9`).

`$sty_inherit`
:   le chemin vers un fichier de style parent.
    
    Si défini, alors le style parent sera utilisé comme base, et ses valeurs
    seront les valeurs par défaut des éléments du style courant.

`$sty_const`
:   Les paramètres propres au style.
    
    C'est un tableau contenant les entrées :
    
    *   COLOR_BLACK : couleur du noir
    *   COLOR_WHITE : couleur du blanc
    *   CORE_BGIMAGE : image de fond de page
    *   CORE_BGCOLOR : couleur du fond d'éléments
    *   CORE_TEXTFGCOLOR : couleur du texte
    *   CORE_TEXTBGCOLOR : couleur de texte contrasté (pour la mise en évidence)
    *   CORE_BGCOLORALTERN : couleur de fond secondaire
    *   CORE_FGCOLOR: couleur de fond utilisé pour le texte contrasté
    *   CORE_BGCOLORHIGH : couleur utilisée pour la mise en évidence
        (généralement *onmouseover*)
    *   CORE_ACOLOR: couleur de hyperliens (ancres)
    *   CORE_BGCELLCOLOR: couleurs des cellules de tableaux
    *   CORE_ERRORCOLOR : couleur des messages d'erreurs

`$sty_local`
:   Les variables à utiliser pour le parsing initial du style.
    
    C'est un tableau. Les clés sont enregistrées comme paramètres volatiles
    le temps de l'initialisation du style.

`$sty_rules`
:   Les règles de style permettant la génération des fichiers _css_ finaux.

### Règles de style _css_ {#core-ref:7ebad29c-1340-4793-b2ee-7bfebc536b1b}

Les règles d'un style définissent comment sont produits les fichiers d'un style.
Ces fichiers peuvent, par exemple, être la simple copie d'un fichier existant,
la concaténation de plusieurs fichiers, la minification d'un fichier, le 
résultat du parsing d'un fichier, une génération complète, etc.

Les règles sont définies au moyen d'un tableau à plusieurs niveaux.

Par exemple :

    [php]
    array(
        'css' => array(
            
            // copie simple
            'dcp/raw/modern.css' => array(
                'src' => 'STYLE/MODERN/Layout/MODERN.css'
            ),
            
            // concaténation simple
            'dcp/raw/ext-adapter.css' => array(
                'src' => array(
                    "extsystem"=>'STYLE/MODERN/Layout/EXT-ADAPTER-SYSTEM.css',
                    "extuser"=>'STYLE/MODERN/Layout/EXT-ADAPTER-USER.css'
                )
            ),
          
            // concaténation avec parsing
            'dcp/parse_on_copy/ext-adapter.css' => array(
                'src' => array(
                    "extsystem"=>'STYLE/MODERN/Layout/EXT-ADAPTER-SYSTEM.css',
                    "extuser"=>'STYLE/MODERN/Layout/EXT-ADAPTER-USER.css'
                ),
                "deploy_parser" => array(
                    "className" => '\Dcp\Style\dcpCssTemplateParser'
                )
            ),
            
            // concaténation simple - 
            // le parsing se fait à la volée lors de la requête
            'dcp/parse_on_load/ext-adapter.css' => array(
                'src' => array(
                    "extsystem"=>'STYLE/MODERN/Layout/EXT-ADAPTER-SYSTEM.css',
                    "extuser"=>'STYLE/MODERN/Layout/EXT-ADAPTER-USER.css'
                ),
                'flags' => Style::RULE_FLAG_PARSE_ON_RUNTIME
            ),
            
            // concaténation avec parsing - 
            // un autre parsing se fait à la volée lors de la requête
            'dcp/parse_twice/ext-adapter.css' => array(
                'src' => array(
                     "extsystem"=>'STYLE/MODERN/Layout/EXT-ADAPTER-SYSTEM.css',
                    "extuser"=>'STYLE/MODERN/Layout/EXT-ADAPTER-USER.css'
                ),
                "deploy_parser" => array(
                    "className" => '\Dcp\Style\dcpCssTemplateParser'
                ),
                'flags' => Style::RULE_FLAG_PARSE_ON_RUNTIME
            )
    
        )
    );

*   L'index de premier niveau correspond au type de fichier produit, parmi :
    *   `css`
        
        Tous les fichiers produits seront placés dans le répertoire `css` à la
        racine du répertoire d'installation.
    
*   L'index de second niveau définit le fichier à produire, au moyen de son
    chemin relatif à la racine pour ce type de fichier (par exemple, pour
    `mypath/myfile.css`, cela correspond à
    `<document root>/css/mypath/myfile.css`).
    
    Le tableau correspondant à cet index définit comment sera générée la cible.
    Les clés significatives de ce tableau sont :
    
    `src`
    :   le ou les fichier(s) source(s), définis par leur chemin relatif au
        *documentroot* ;
    
    `flags`
    :   un masque binaire indiquant des options de génération.
        
        les flags actuellement disponibles sont :
        
        `Style::RULE_FLAG_PARSE_ON_DEPLOY`
        :   Indique que les sources seront envoyées à un parser
            lors de leur *déploiement*
        
        `Style::RULE_FLAG_PARSE_ON_RUNTIME`
        :   Indique que les sources seront envoyées à un parser
            lors de leur *téléchargement par le client*
    
    `deploy_parser`
    :   La configuration du parser à utiliser pour le déploiement (voir [les
        parsers][parsers]).
        
        C'est un tableau contenant :
        
        *   à l'index `className` le nom de la classe (namespaces inclus)
            à utiliser pour produire le fichier
            
            en l'absence de valeur, c'est la classe 
            `\Dcp\Style\dcpCssConcatParser` qui est utilisée.
        
        *   à l'index `options` un tableau d'options qui sera envoyé au parser.
    
    `runtime_parser`
    :   La configuration du parser à utiliser pour le déploiement (voir [les
        parsers][parsers]).
        
        **Pour le moment, le parsing au runtime ne supporte pas de classe
        personnalisée.  
        Si `Style::RULE_FLAG_PARSE_ON_RUNTIME` est positionné,
        le fichier sera parsé par le moteur de template de Dynacase.  
        Aussi, positionner une valeur autre que `null` à l'index className
        lèvera une erreur.**
        
        C'est un tableau contenant :
        
        *   à l'index `className` le nom de la classe (namespaces inclus)
            à utiliser pour produire le fichier.
        
        *   à l'index `options` un tableau d'options qui sera envoyé au parser.

#### Règles locales {#core-ref:e88dc31c-91b1-4fa3-9610-a959e53512d3}

Les règles locales sont définies dans la variable `$sty_rules` du fichier de 
définition de style.

#### Surcharge de règles {#core-ref:1e6608ed-fd58-4865-8a4c-58f3c4ce60a5}

Lors de son déploiement, le style récupère également toutes les règles 
définies dans les fichiers contenus dans le répertoire `rules.d` 
placé à la racine du style. Ces fichiers sont chargés par ordre alphabétique,
et seule leur variable `$sty_rules` est utilisée, afin de surcharger les règles
locales.

Cela permet notamment

*   de rajouter des règles à un style déjà déployé,
*   d'altérer les règles d'un style déjà déployé.

### Parsers {#core-ref:f5f9836a-a596-4a12-afab-ea23de37ab16}

Les parsers sont des classes implémentant l'interface `\Dcp\Style\IParser` et
celle correspondant au type de fichier produit (pour les css,
`\Dcp\Style\ICssParser`).

Ils implémentent donc

*   un constructeur prenant en entrée le(s) fichier(s) source(s),
    un tableau d'options, ainsi que la configuration complète du style.
*   une méthode `gen` prenant en paramètre le chemin du fichier à produire.

Les parsers fournis par défaut sont :

`\Dcp\Style\dcpCssConcatParser`
:   un parser faisant uniquement la concaténation de css

`\Dcp\Style\dcpCssTemplateParser`
:   un parser faisant la concaténation de css, puis les parsant au moyen
    du moteur de template de Dynacase.
    
    Les parsers étant chargés au moyen de l'autoloader, il est parfaitement
    possible de définir de nouvelles classes de parser, tant qu'elles
    implémentent les interfaces correspondantes.

`\Dcp\Style\dcpCssCopyDirectory`
:   un parser permettant de copier le contenu d'un répertoire. Ceci est utilisé
    notamment pour copier un ensemble d'images liées à la css.

`\Dcp\Style\dcpLessParser`<span class="flag fixme">à compléter / @import </span>
:   un parser Less générant les Css à partir du source .less

## Créer un nouveau style à partir du style par défaut {#core-ref:5df70d18-c735-48d4-8444-53d0cf6880b2}

Le module "Dynacase-core" livre 3 styles :

*   `DEFAULT` : Le style de référence
*   `MODERN` : Le style installé par défaut. Il hérite de `DEFAULT`
*   `ORIGINAL` : Ancien style gardé pour compatibilité. Il hérite de `DEFAULT`

### Définition des règles par défaut {#core-ref:42fbc102-c01a-4a08-98f7-d1610f8441ba}

Les règles par défaut sont celles décrites dans le style `DEFAULT`.

#### Règles interfaces initiales {#core-ref:76671db7-f66c-4a6f-853e-e573be03f213}
Les interfaces standards de dynacase utilisent les _css_ suivantes :

'dcp/main.css'
:   Cette _css_ est utilisée dans la plupart des interfaces documentaires de haut
    niveau fournies par le module _dynacase-core_.  
    Elle est utilisée pour les interfaces web de documents (consultation et
    modification) et pour les listes de documents.
    
    Elle est composée des éléments suivants :
    
    *   `core` => "CORE/Layout/core.css",
    *   `fdl` => "FDL/Layout/freedom.css",
    *   `size` => "WHAT/Layout/size-normal.css",
    *   `style` => "STYLE/DEFAULT/Layout/DEFAULT.css"

`dcp/core.css`
:   Cette _css_ est utilisée par défaut sur les interfaces si le template utilisé
    possède la clef `[CSS:REF]`.  
    Elle est utilisée sur les interfaces de core d'importation et de gestion des
    droits.
    
    Elle est composée des éléments suivants :
    
    *   `core` => "CORE/Layout/core.css",
    *   `size` => "WHAT/Layout/size-normal.css",
    *   `style` => "STYLE/DEFAULT/Layout/DEFAULT.css"

'dcp/document-view.css' 
:   Cette _css_ est utilisée sur les vues standards de consultation des documents.
    La vue de consultation utilise aussi la _css_ `dcp/main.css`.
    
    Elle est composée des éléments suivants :
    
    * `document` => "FDL/Layout/document.css"

`dcp/document-edit.css`
:   Cette _css_ est utilisée sur les vues standards de modification des documents
    
    Elle est composée des éléments suivants :
    
    *   `edit` => "FDL/Layout/editdoc.css",
    *   `autocompletion` => "FDL/Layout/autocompletion.css",
    *   `popup` => "FDL/Layout/popup.css",
    *   `document` => "FDL/Layout/document.css",
    *   `jscalendar` => "jscalendar/Layout/calendar-win2k-2.css"

`dcp/system.css`
:   Cette _css_ est utilisée par défaut sur les interfaces si le template utilisé
    possède la clef `[CSS:REF]`. Elle est utilisée sur les anciennes interfaces
    de core.
    
    Elle est composée des éléments suivants :
    
    *   `size` => "WHAT/Layout/size-normal.css",
    *   `style` => "STYLE/DEFAULT/Layout/DEFAULT.css"

Toutes ces règles utilisent le parser `dcpCssTemplateParse`.

#### Règles interfaces jquery-ui {#core-ref:c0d92391-7a86-4cee-a844-ac0e3c686e88}

Le module `dynacase-jqueryui-installer` ajoute aussi deux autres règles au style
`DEFAULT` :

 `dcp/jquery-ui.css`
 :  Cette _css_ est utilisée dans les interfaces utilisant jquery-ui sauf dans
    l'interface GENERIC_LIST (liste de documents de ONEFAM) qui utilise une
    autre version de jquery-ui.
    
    Elle est composée des éléments suivants :
    
    * `jqueryui` => "lib/jquery-ui/css/smoothness/jquery-ui.css"
    
    Cette règle n'utilise pas de parser.

`dcp/images`
:   Cette règles permet de copier les images utiles à jquery-ui
    Elle utilise les images suivantes :
    
    * `jqueryimages` => "lib/jquery-ui/css/smoothness/images"
    
    Cette règle utilise le parser `dcpCssCopyDirectory`


#### Description du fichier `css` {#core-ref:79113a72-646a-4cfd-a60e-bb43dd28faf1}

Le fichier `css` de style est utilisable dans les règles.

En l'absence de parser, ce fichier doit être autonome.

Il doit être composé uniquement de règles _css_ valides.

Les url d'accès aux images doivent être définies par rapport à la cible (par
exemple, si la cible est `mypath/mycss.css` le fichier produit sera placé dans
le répertoire `css/mypath/mycss.css`).

Si le parser employé est `dcpCssTemplateParser` alors les variables définies
dans `$sty_const` et `$sty_local` seront utilisables.

Voici un exemple de production de _css_ avec le fichier de style suivant :

    [php]
    $sty_const = array(
        "COLOR_BLACK" => "#000000",
        "COLOR_WHITE" => "#FFFFFF"
    );
    // style color variable to compute colors
    // COLOR_A0 -> COLOR_A9
    // COLOR_B0 -> COLOR_B9
    // COLOR_C0 -> COLOR_C9
    $sty_colors = array(
        "A" => '#4D4D4D', // gray
        "B" => '#126ab5',  //blue
        "C" => "#cbc11f"// gold
    ); 
    // local variable for compose style css
    $sty_local = array(
        "input-text-normal-color" => "COLOR_BLACK",
        "input-text-hover-color" => "yellow",


De plus les 10 nuances de couleurs COLOR_A, COLOR_B et COLOR_C peuvent aussi
être utilisées.

<table>
    <tr>
        <td style='background-color:#4D4D4D'>COLOR_A0: #4D4D4D</td>
        <td style='background-color:#5F5F5F'>COLOR_A1: #5F5F5F</td>
        <td style='background-color:#717171'>COLOR_A2: #717171</td>
        <td style='background-color:#828282'>COLOR_A3: #828282</td>
        <td style='background-color:#949494'>COLOR_A4: #949494</td>
        <td style='background-color:#A6A6A6'>COLOR_A5: #A6A6A6</td>
        <td style='background-color:#B8B8B8'>COLOR_A6: #B8B8B8</td>
        <td style='background-color:#CACACA'>COLOR_A7: #CACACA</td>
        <td style='background-color:#DBDBDB'>COLOR_A8: #DBDBDB</td>
        <td style='background-color:#EDEDED'>COLOR_A9: #EDEDED</td>
    </tr>
    <tr>
        <td style='background-color:#126AB5'>COLOR_B0: #126AB5</td>
        <td style='background-color:#157BD1'>COLOR_B1: #157BD1</td>
        <td style='background-color:#1D8BE8'>COLOR_B2: #1D8BE8</td>
        <td style='background-color:#3999EB'>COLOR_B3: #3999EB</td>
        <td style='background-color:#55A8EE'>COLOR_B4: #55A8EE</td>
        <td style='background-color:#72B6F1'>COLOR_B5: #72B6F1</td>
        <td style='background-color:#8EC5F4'>COLOR_B6: #8EC5F4</td>
        <td style='background-color:#AAD3F7'>COLOR_B7: #AAD3F7</td>
        <td style='background-color:#C6E2F9'>COLOR_B8: #C6E2F9</td>
        <td style='background-color:#E3F0FC'>COLOR_B9: #E3F0FC</td>
    </tr>
    <tr>
        <td style='background-color:#CBC11F'>COLOR_C0: #CBC11F</td>
        <td style='background-color:#DED328'>COLOR_C1: #DED328</td>
        <td style='background-color:#E2D83F'>COLOR_C2: #E2D83F</td>
        <td style='background-color:#E5DD57'>COLOR_C3: #E5DD57</td>
        <td style='background-color:#E9E26F'>COLOR_C4: #E9E26F</td>
        <td style='background-color:#EDE787'>COLOR_C5: #EDE787</td>
        <td style='background-color:#F0EC9F'>COLOR_C6: #F0EC9F</td>
        <td style='background-color:#F4F0B7'>COLOR_C7: #F4F0B7</td>
        <td style='background-color:#F8F5CF'>COLOR_C8: #F8F5CF</td>
        <td style='background-color:#FBFAE7'>COLOR_C9: #FBFAE7</td>
    </tr>
</table>


Pour un fichier _css_ comme décrit ci-dessous :

    [css]
    input : {
        color: [input-text-normal-color];
        background-color: [COLOR_WHITE];
    }
    input:hover : {
        color: [input-text-hover-color];
        background-color: [COLOR_C5];
    }

Le fichier _css_ produit sera :

    [css]
    input : {
        color: #000000;
        background-color: #ffffff;
    }
    input:hover : {
        color: yellow;
        background-color: #EDE787;
    }


#### Fichier `css` avec flag RULE_FLAG_PARSE_ON_RUNTIME {#core-ref:6135aac5-c209-4971-960d-97129c24ac49}

Si la règle utilise le flag `Style::RULE_FLAG_PARSE_ON_RUNTIME` alors la css
peut aussi utiliser les variables suivantes :

*   "ISIE" : vrai si le navigateur est [Internet Explorer][MSIE]
*   "ISIE6", vrai si le navigateur est une version 6 d'Internet Explorer
*   "ISIE7", vrai si le navigateur est une version 7 d'Internet Explorer
*   "ISIE8", vrai si le navigateur est une version 8 d'Internet Explorer
*   "ISIE9", vrai si le navigateur est une version 9 d'Internet Explorer
*   "ISIE10",vrai si le navigateur est une version 10 d'Internet Explorer
*   "ISAPPLEWEBKIT": vrai si le navigateur est basé sur [Apple Web Kit][WEBKIT]
    (Chrome et Safari entre autre)
*   "ISSAFARI" : vrai si le navigateur est basé sur [Safari][SAFARI]
*   "ISCHROME" : vrai si le navigateur est basé sur [Google Chrome][CHROME]

Ces variables sont basées sur le _User-Agent_ envoyé par le navigateur.

Le fichier _css_ peut avoir des parties conditionnelles :

    [css]
    input : {
        color: [input-text-normal-color];
        background-color: [COLOR_WHITE];
        [IF ISIE]padding:10px;[ENDIF ISIE]
        [IFNOT ISCHROME]margin:10px;[ENDIF ISCHROME]
    }

Les règles suivantes :

*   `dcp/main.css`,
*   `dcp/core.css`,
*   `dcp/document-edit.css`,
*   `dcp/system.css`,

utilisent le flag `RULE_FLAG_PARSE_ON_RUNTIME`.

### Surcharge de `DEFAULT` {#core-ref:8d244db4-2757-4e00-83e3-adaff8f258dd}


#### Définir son fichier de style avec héritage  {#core-ref:614d39c9-6dc4-4af6-9749-cd3a66b2da0e}
Pour surcharger le style `DEFAULT` il faut dans le fichier de style `.sty` l'héritage :

    MONSTYLE.sty

    [php]
    $sty_inherit = "STYLE/DEFAULT/DEFAULT.sty";

Ensuite il faut créer son fichier MONSTYLE.css qui doit être dans le répertoire
`STYLE/MONSTYLE/Layout`.

Les règles de surcharge de style sont :

    [php]
    $sty_rules = array(
        'css' => array(
            'dcp/main.css' => array(
                "src" => array(
                    "style" => "STYLE/MONSTYLE/Layout/MONSTYLE.css"
                )
            ) ,
            'dcp/core.css' => array(
                "src" => array(
                    "style" => "STYLE/MONSTYLE/Layout/MONSTYLE.css"
                )
            ) ,
            'dcp/document-edit.css' => array(
                "src" => array(
                    "jscalendar" => "STYLE/MONSTYLE/Layout/calendar.css"
                )
            ) ,
            'dcp/system.css' => array(
                "src" => array(
                    "style" => "STYLE/MONSTYLE/Layout/MONSTYLE.css"
                )
            )
        )
    );

L'exemple remplace le fichier `DEFAULT.css` par `MONSTYLE.css` en réutilisant
l'index `style` des sources. Il remplace aussi la _css_ du calendrier utilisé
dans la modification de documents.



Il est aussi possible d'ajouter et non de remplacer des éléments de _css_ :

    [php]
    $sty_desc = array(
        "name" => "SKYBLUE", //Name
        "description" => N_("Blue Style") , //long description 
    );
    
    $sty_inherit = "STYLE/MODERN/MODERN.sty";
    
    $sty_rules = array(
        'css' => array(
            'dcp/jquery-ui.css' => array(
                "src" => array(
                    "blue"=>"STYLE/SKYBLUE/Layout/jquery-blue.css"
                )
            ) ,
            'dcp/main.css' => array(
                "src" => array(
                    "blue"=>"STYLE/SKYBLUE/Layout/main-blue.css"
                )
            )
        )
    );

Cette exemple reprend le style `MODERN` (fourni par _dynacase-core_) qui hérite
lui-même de `DEFAULT`. Dans ce dernier exemple la règle `dcp/jquery-ui.css`
sera composée de :

* `jqueryui` => "lib/jquery-ui/css/smoothness/jquery-ui.css"
* `blue`=>"STYLE/SKYBLUE/Layout/jquery-blue.css"

## Créer un nouveau style sans héritage {#core-ref:96d4a839-233a-4204-9e5d-a2da6a8b66e4}

Pour créer un style sans héritage il est nécessaire de déclarer l'ensemble des
 règles nécessaires aux différentes interfaces :

*   dcp/core.css
*   dcp/document-edit.css
*   dcp/document-view.css
*   dcp/jquery-ui.css
*   dcp/main.css
*   dcp/system.css

## Surcharger des styles

Il est possible d'ajouter des règles globales qui seront systématiquement ajoutéees à la définition des styles.
Ces règles seront prise en compte lors de la génération dynamique du thème quel que soit le style appliqué.

Pour cela, il vous faut créer un fichier `.sty` dans le répertoire `STYLES/global-rules.d/`.

La syntaxe est la même que celle vue aux chapitres précédents.

Par exemple, le fichier `STYLES/global-rules.d/monapp.sty` :

    [php]
    $sty_rules = array(
        'css' => array(
            'css/monapp/main.css' => array(
                "src" => array(
                    "style" => "MONAPP/Layout/monapp.less"
                )
            ),
            "deploy_parser" => array(
                "className" => '\Dcp\Style\dcpLessParser',
                "options" => array(
                    "sourceMap" => true
                )
            )
        ),
        'css' => array(
            'css/monapp/colors.css' => array(
                "src" => array(
                    "blue"=>"MONAPP/Layout/colors.css"
                )
            )
        )
    );

La génération du thème à partir du style va produire les CSS :

* `css/monapp/main.css`, générée par le parser `Less` à partir du source `MONAPP/Layout/monapp.less`
* `css/monapp/colors.css`

## Surcharger les templates {#core-ref:dc09a573-64be-4ec6-99a2-2166016a3387}

Au delà des css, le style peut aussi surcharger les templates des interfaces
des actions de Dynacase. Les templates doivent se trouver dans le répertoire
`Layout` du style.

Par exemple, l'interface de connexion utilise l'action `AUTHENT:LOGINFORM`.

Cette action utilise le template `AUTHENT/Layout/loginform.xml`. Si le style
`MONSTYLE` publie  le fichier `STYLE/MONSTYLE/Layout/loginform.xml` alors
l'action `AUTHENT:LOGINFORM`  utilisera le template du style pour l'interface de
connexion si le style `MONSTYLE` est installé.

## Surcharger les images {#core-ref:10bb3d26-f517-491a-95a6-6449152fb220}

De même, les images peuvent être surchargées. Pour cela il faut publier des
images de même nom dans le répertoire `Images` du style.

Par exemple la famille _Groupe d'utilisateurs_ utilise l'image `igroup.png`
comme icone. Si le style `MONSTYLE` publie l'image
`STYLE/MONSTYLE/Images/igroup.png`, alors l'icone de la famille sera changée.

La surcharge d'image ne fonctionne que si les templates ont utilisé la balise 
`[IMG]` ou la méthode `Action::getImageUrl()`.

**Attention** : Le cache du navigateur peut conserver les anciennes images.

## Installer un style {#core-ref:558881a8-8fcb-411e-a0e9-0178053839bb}

Une fois l'arborescence de fichiers installée sur le serveur, il faut utiliser
la commande `setStyle` depuis la console.

    $ ./wsh.php --api=setStyle --style=./STYLE/MONSTYLE/MONSTYLE.sty

Cette commande permet d'appliquer les règles définies dans le style. Les
fichiers _css_ sont alors produits dans le répertoire `css` à la racine du
répertoire d'installation.

L'option `--verbose=yes` permet de voir quelles sont les règles prises en compte.

Si le paramètre `style` n'est pas indiqué, alors la commande `setStyle` va
régénérer les fichiers composants le style courant.

<!-- links -->
[MSIE]: http://fr.wikipedia.org/wiki/Internet_Explorer "Description de Internet Explorer sur wikipedia"
[WEBKIT]: http://fr.wikipedia.org/wiki/WebKit "Description de WebKit sur wikipedia"
[SAFARI]: http://fr.wikipedia.org/wiki/Safari_%28logiciel%29- "Description de Safari sur wikipedia"
[CHROME]: http://fr.wikipedia.org/wiki/Google_Chrome "Description de Google Chrome sur wikipedia"
[parsers]: #core-ref:f5f9836a-a596-4a12-afab-ea23de37ab16