# Les attributs calculés {#core-ref:4565cab9-73c8-4eee-bfa7-218ffbd4b687}
 
## Présentation {#core-ref:2e0f2949-eccc-4485-915c-3953f170da86}

Un *attribut calculé* est un attribut dont la valeur est calculée par Dynacase.
Ce calcul est effectué à chaque appel de la méthode `Doc::refresh()`, qui est
appelée très fréquemment sur les documents. Dans de nombreux cas, il sera
préférable d'utiliser les [hooks du document][hooks] (en particulier
`Doc::postStore`).

Le calcul de l'attribut peut être

*   local
    
    Par exemple : *attr_c = attr_a * attr_b*

*   global
    
    Par exemple : *attr_d = nombre de documents du même auteur*

*   externe
    
    Par exemple : *attr_e = quantité en stock dans l'ERP*

Ce calcul peut être effectué au moyen :

*    d'une méthode *publique* de la classe du document contenant l'attribut,
*    d'une méthode statique d'une autre classe.

Elle doit retourner un résultat sous forme d'une chaîne de caractères.
Le résultat est mis dans l'attribut sur lequel s'applique le calcul
(sauf redirection explicite faite lors de la déclaration de l'attribut).

Le calcul est effectué lors du rafraîchissement des documents (voir la méthode
[`Doc::refresh()`][refresh]).

Les attributs en visibilité `I` ne sont pas modifiés lors de l'actualisation.
Pour que ces attributs soit modifiés, il est nécessaire qu'un contrôle de vue
modifie cette visibilité pour l'utilisateur courant. Si l'attribut est en
visibilité `I` au moment du calcul, un message d'avertissement sera écrit dans
les [logs système][warning].



D'un point de vue utilisateur, un attribut calculé est rafraîchi avant chaque
consultation de document.

## Syntaxe {#core-ref:bb54f32d-d2dd-4702-9c92-e442c800263c}

La syntaxe définissant les attributs calculés est la suivante :

<div class="grammar">

<svg width="544" height="132" viewBox="-8 -58 544 132 " xmlns="http://www.w3.org/2000/svg" version="1.1">
<defs><style type="text/css">.terminal_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.terminal_text{fill:black;font-family:Verdana,Sans-serif;text-anchor:middle;font-size:14px;}.symbol_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.symbol_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-style:italic;text-anchor:middle;font-size:14px;}.path{fill:none;stroke:black;stroke-width:2;}.rule_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-size:14px;}.rule_path_edge{fill:none;stroke:black;stroke-width:3;}</style></defs>
<text class="rule_text" x="0" y="-36" >computeMethod:</text>
<rect class="symbol_rect" x="30" y="-14" width="116" height="28" rx="1" />
<text class="symbol_text" x="88" y="4" >callMethod</text>
<rect class="terminal_rect" x="206" y="-14" width="26" height="28" rx="28" />
<text class="terminal_text" x="219" y="4" >:</text>
<rect class="symbol_rect" x="256" y="-14" width="206" height="28" rx="1" />
<text class="symbol_text" x="359" y="4" >targetAttributeName</text>
<path class="path" d="M232 0 L256 0" />
<path class="path" d="M206 44 L222 44" />
<path class="path" d="M170 0 L206 0 M170 0 Q182 0 182 12 M498 0 L462 0 M498 0 Q486 0 486 12" />
<path class="path" d="M194 44 L206 44 M194 44 Q182 44 182 32 L182 12 M474 44 L222 44 M474 44 Q486 44 486 32 L486 12" />
<path class="path" d="M146 0 L170 0" />
<path class="path" d="M0 0 L30 0 M528 0 L498 0" />
<path class="rule_path_edge" d="M0 -6 L0 6 M528 -6 L528 6" />
</svg>

<svg width="806" height="132" viewBox="-8 -58 806 132 " xmlns="http://www.w3.org/2000/svg" version="1.1">
<defs><style type="text/css">.terminal_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.terminal_text{fill:black;font-family:Verdana,Sans-serif;text-anchor:middle;font-size:14px;}.symbol_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.symbol_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-style:italic;text-anchor:middle;font-size:14px;}.path{fill:none;stroke:black;stroke-width:2;}.rule_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-size:14px;}.rule_path_edge{fill:none;stroke:black;stroke-width:3;}</style></defs>
<text class="rule_text" x="0" y="-36" >callMethod:</text>
<rect class="symbol_rect" x="66" y="-14" width="126" height="28" rx="1" />
<text class="symbol_text" x="129" y="4" >staticClass</text>
<path class="path" d="M66 44 L82 44" />
<path class="path" d="M30 0 L66 0 M30 0 Q42 0 42 12 M228 0 L192 0 M228 0 Q216 0 216 12" />
<path class="path" d="M54 44 L66 44 M54 44 Q42 44 42 32 L42 12 M204 44 L82 44 M204 44 Q216 44 216 32 L216 12" />
<rect class="terminal_rect" x="252" y="-14" width="36" height="28" rx="28" />
<text class="terminal_text" x="270" y="4" >::</text>
<rect class="symbol_rect" x="312" y="-14" width="116" height="28" rx="1" />
<text class="symbol_text" x="370" y="4" >methodName</text>
<rect class="terminal_rect" x="452" y="-14" width="26" height="28" rx="28" />
<text class="terminal_text" x="465" y="4" >(</text>
<rect class="symbol_rect" x="538" y="-14" width="136" height="28" rx="1" />
<text class="symbol_text" x="606" y="4" >methodInputs</text>
<path class="path" d="M538 44 L554 44" />
<path class="path" d="M502 0 L538 0 M502 0 Q514 0 514 12 M710 0 L674 0 M710 0 Q698 0 698 12" />
<path class="path" d="M526 44 L538 44 M526 44 Q514 44 514 32 L514 12 M686 44 L554 44 M686 44 Q698 44 698 32 L698 12" />
<rect class="terminal_rect" x="734" y="-14" width="26" height="28" rx="28" />
<text class="terminal_text" x="747" y="4" >)</text>
<path class="path" d="M228 0 L252 0" />
<path class="path" d="M288 0 L312 0" />
<path class="path" d="M428 0 L452 0" />
<path class="path" d="M478 0 L502 0" />
<path class="path" d="M710 0 L734 0" />
<path class="path" d="M0 0 L30 0 M790 0 L760 0" />
<path class="rule_path_edge" d="M0 -6 L0 6 M790 -6 L790 6" />
</svg>

<svg width="498" height="144" viewBox="-8 -86 498 144 " xmlns="http://www.w3.org/2000/svg" version="1.1">
<defs><style type="text/css">.terminal_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.terminal_text{fill:black;font-family:Verdana,Sans-serif;text-anchor:middle;font-size:14px;}.symbol_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.symbol_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-style:italic;text-anchor:middle;font-size:14px;}.path{fill:none;stroke:black;stroke-width:2;}.rule_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-size:14px;}.rule_path_edge{fill:none;stroke:black;stroke-width:3;}</style></defs>
<text class="rule_text" x="0" y="-64" >methodInputs:</text>
<rect class="symbol_rect" x="30" y="-14" width="126" height="28" rx="1" />
<text class="symbol_text" x="93" y="4" >methodInput</text>
<rect class="terminal_rect" x="228" y="-14" width="26" height="28" rx="28" />
<text class="terminal_text" x="241" y="4" >,</text>
<rect class="symbol_rect" x="278" y="-14" width="126" height="28" rx="1" />
<text class="symbol_text" x="341" y="4" >methodInput</text>
<path class="path" d="M254 0 L278 0" />
<path class="path" d="M228 0 L204 0 M216 0 Q204 0 204 -12 L204 -36 Q204 -48 216 -48 L416 -48 Q428 -48 428 -36 L428 -12 Q428 0 416 0 L404 0 L428 0" />
<path class="path" d="M228 0 L180 0 Q192 0 192 12 L192 24 Q192 36 204 36 L428 36 Q440 36 440 24 L440 12 Q440 0 452 0 L404 0 M180 0 L204 0 M428 0 L452 0" />
<path class="path" d="M156 0 L180 0" />
<path class="path" d="M0 0 L30 0 M482 0 L452 0" />
<path class="rule_path_edge" d="M0 -6 L0 6 M482 -6 L482 6" />
</svg>

<svg width="786" height="248" viewBox="-8 -86 786 248 " xmlns="http://www.w3.org/2000/svg" version="1.1">
<defs><style type="text/css">.terminal_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.terminal_text{fill:black;font-family:Verdana,Sans-serif;text-anchor:middle;font-size:14px;}.symbol_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.symbol_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-style:italic;text-anchor:middle;font-size:14px;}.path{fill:none;stroke:black;stroke-width:2;}.rule_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-size:14px;}.rule_path_edge{fill:none;stroke:black;stroke-width:3;}</style></defs>
<text class="rule_text" x="0" y="-64" >methodInput:</text>
<rect class="terminal_rect" x="78" y="-14" width="96" height="28" rx="28" />
<text class="terminal_text" x="126" y="4" >&lt;spaces&gt;</text>
<path class="path" d="M78 0 L54 0 M66 0 Q54 0 54 -12 L54 -36 Q54 -48 66 -48 L186 -48 Q198 -48 198 -36 L198 -12 Q198 0 186 0 L174 0 L198 0" />
<path class="path" d="M78 0 L30 0 Q42 0 42 12 L42 24 Q42 36 54 36 L198 36 Q210 36 210 24 L210 12 Q210 0 222 0 L174 0 M30 0 L54 0 M198 0 L222 0" />
<rect class="symbol_rect" x="282" y="-14" width="206" height="28" rx="1" />
<text class="symbol_text" x="385" y="4" >sourceAttributeName</text>
<rect class="symbol_rect" x="282" y="30" width="206" height="28" rx="1" />
<text class="symbol_text" x="385" y="48" >familyParameterName</text>
<rect class="symbol_rect" x="282" y="74" width="56" height="28" rx="1" />
<text class="symbol_text" x="310" y="92" >text</text>
<rect class="symbol_rect" x="282" y="118" width="86" height="28" rx="1" />
<text class="symbol_text" x="325" y="136" >keyWord</text>
<path class="path" d="M246 0 L282 0 M246 0 Q258 0 258 12 M524 0 L488 0 M524 0 Q512 0 512 12" />
<path class="path" d="M270 44 L282 44 M270 44 Q258 44 258 32 L258 12 M500 44 L488 44 M500 44 Q512 44 512 32 L512 12" />
<path class="path" d="M270 88 L282 88 M270 88 Q258 88 258 76 L258 32 M500 88 L338 88 M500 88 Q512 88 512 76 L512 32" />
<path class="path" d="M270 132 L282 132 M270 132 Q258 132 258 120 L258 76 M500 132 L368 132 M500 132 Q512 132 512 120 L512 76" />
<rect class="terminal_rect" x="596" y="-14" width="96" height="28" rx="28" />
<text class="terminal_text" x="644" y="4" >&lt;spaces&gt;</text>
<path class="path" d="M596 0 L572 0 M584 0 Q572 0 572 -12 L572 -36 Q572 -48 584 -48 L704 -48 Q716 -48 716 -36 L716 -12 Q716 0 704 0 L692 0 L716 0" />
<path class="path" d="M596 0 L548 0 Q560 0 560 12 L560 24 Q560 36 572 36 L716 36 Q728 36 728 24 L728 12 Q728 0 740 0 L692 0 M548 0 L572 0 M716 0 L740 0" />
<path class="path" d="M222 0 L246 0" />
<path class="path" d="M524 0 L548 0" />
<path class="path" d="M0 0 L30 0 M770 0 L740 0" />
<path class="rule_path_edge" d="M0 -6 L0 6 M770 -6 L770 6" />
</svg>

<svg width="324" height="176" viewBox="-8 -58 324 176 " xmlns="http://www.w3.org/2000/svg" version="1.1">
<defs><style type="text/css">.terminal_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.terminal_text{fill:black;font-family:Verdana,Sans-serif;text-anchor:middle;font-size:14px;}.symbol_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.symbol_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-style:italic;text-anchor:middle;font-size:14px;}.path{fill:none;stroke:black;stroke-width:2;}.rule_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-size:14px;}.rule_path_edge{fill:none;stroke:black;stroke-width:3;}</style></defs>
<text class="rule_text" x="0" y="-36" >text:</text>
<rect class="terminal_rect" x="66" y="-14" width="26" height="28" rx="28" />
<text class="terminal_text" x="79" y="4" >&apos;</text>
<rect class="terminal_rect" x="116" y="-14" width="76" height="28" rx="28" />
<text class="terminal_text" x="154" y="4" >&lt;text&gt;</text>
<path class="path" d="M92 0 L116 0" />
<rect class="terminal_rect" x="66" y="30" width="26" height="28" rx="28" />
<text class="terminal_text" x="79" y="48" >&apos;</text>
<rect class="terminal_rect" x="116" y="30" width="76" height="28" rx="28" />
<text class="terminal_text" x="154" y="48" >[^&apos;,]+</text>
<rect class="terminal_rect" x="216" y="30" width="26" height="28" rx="28" />
<text class="terminal_text" x="229" y="48" >&apos;</text>
<path class="path" d="M92 44 L116 44" />
<path class="path" d="M192 44 L216 44" />
<rect class="terminal_rect" x="66" y="74" width="26" height="28" rx="28" />
<text class="terminal_text" x="79" y="92" >&quot;</text>
<rect class="terminal_rect" x="116" y="74" width="76" height="28" rx="28" />
<text class="terminal_text" x="154" y="92" >[^&quot;,]+</text>
<rect class="terminal_rect" x="216" y="74" width="26" height="28" rx="28" />
<text class="terminal_text" x="229" y="92" >&quot;</text>
<path class="path" d="M92 88 L116 88" />
<path class="path" d="M192 88 L216 88" />
<path class="path" d="M30 0 L66 0 M30 0 Q42 0 42 12 M278 0 L192 0 M278 0 Q266 0 266 12" />
<path class="path" d="M54 44 L66 44 M54 44 Q42 44 42 32 L42 12 M254 44 L242 44 M254 44 Q266 44 266 32 L266 12" />
<path class="path" d="M54 88 L66 88 M54 88 Q42 88 42 76 L42 32 M254 88 L242 88 M254 88 Q266 88 266 76 L266 32" />
<path class="path" d="M0 0 L30 0 M308 0 L278 0" />
<path class="rule_path_edge" d="M0 -6 L0 6 M308 -6 L308 6" />
</svg>

<svg width="204" height="132" viewBox="-8 -58 204 132 " xmlns="http://www.w3.org/2000/svg" version="1.1">
<defs><style type="text/css">.terminal_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.terminal_text{fill:black;font-family:Verdana,Sans-serif;text-anchor:middle;font-size:14px;}.symbol_rect{fill:rgb(206,255,206);stroke:black;stroke-width:2;}.symbol_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-style:italic;text-anchor:middle;font-size:14px;}.path{fill:none;stroke:black;stroke-width:2;}.rule_text{fill:black;font-family:Verdana,Sans-serif;font-weight:bold;font-size:14px;}.rule_path_edge{fill:none;stroke:black;stroke-width:3;}</style></defs>
<text class="rule_text" x="0" y="-36" >keyWord:</text>
<rect class="terminal_rect" x="66" y="-14" width="56" height="28" rx="28" />
<text class="terminal_text" x="94" y="4" >THIS</text>
<rect class="terminal_rect" x="66" y="30" width="26" height="28" rx="28" />
<text class="terminal_text" x="79" y="48" >K</text>
<path class="path" d="M30 0 L66 0 M30 0 Q42 0 42 12 M158 0 L122 0 M158 0 Q146 0 146 12" />
<path class="path" d="M54 44 L66 44 M54 44 Q42 44 42 32 L42 12 M134 44 L92 44 M134 44 Q146 44 146 32 L146 12" />
<path class="path" d="M0 0 L30 0 M188 0 L158 0" />
<path class="rule_path_edge" d="M0 -6 L0 6 M188 -6 L188 6" />
</svg>

</div>

Ce qui donne en [BNF][WP_BNF] :

    computeMethod   ::= callMethod ( ( ':' targetAttributeName ) |  )
    
    callMethod      ::= ( staticClass |  ) '::' methodName '(' ( methodInputs |  ) ')'
    
    methodInputs    ::= methodInput ( ',' methodInput )*
    
    methodInput     ::= '<spaces>'* ( sourceAttributeName | familyParameterName | text | keyWord ) '<spaces>'*
    
    text            ::= ( "'" '<text>' ) | ( "'" "[^',]+" "'" ) | ( '"' '[^",]+' '"' )
    
    keyWord         ::= 'THIS' | 'K'

avec les éléments suivants :

targetAttributeName
:   Un nom d'attribut existant dans la famille.
    
    Le résultat de la méthode sera affecté à cet attribut.
    
    Si *targetAttributeName* n'est pas renseigné, alors c'est l'attribut
    sur lequel est définie la méthode de calcul qui reçoit la valeur.

sourceAttributeName
:   Un nom d'attribut existant dans la famille.
    
    La méthode de calcul recevra la valeur de l'attribut au moment de l'appel.
    
    Lorsque *attributeName* est dans un array, alors la valeur passée est :
    
    *   la valeur sur la même ligne si l'attribut calculé est dans le même tableau
    *   la valeur de la colonne (sous la forme d'un tableau php) si l'attribut
        calculé est en dehors du tableau
    *   la valeur sur la même ligne  si l'attribut est dans un autre tableau, ce
        qui peut conduire à des comportements surprenants.

familyParametername
:   Un nom de paramètre existant dans la famille.
    
    La fonction de calcul recevra la valeur du paramètre au moment de l'appel.

keyWord
:   Un mot clé qui est remplacé par une valeur dynamique
    
    THIS
    :   L'objet du document en cours.
        
        Cela est utile dans le cas d'un appel à une méthode statique.
    
    K
    :   L'index de la ligne en cours si l'attribut est dans un tableau.
        
        Si l'attribut n'est pas dans un tableau, la valeur passée est `-1`

## Exemples {#core-ref:f2823f33-e95d-4fdc-be56-efa5a19a8c4c}

### Calcul d'un prix total {#core-ref:e9d8d096-3c5c-4f99-afd8-6365e561d75f}

Extrait de la définition de la famille

| attrid                | label         | type   | visibility | phpfunc                                             |
| -                     |               |        |            |                                                     |
| article_qte           | Quantité      | int    | W          |                                                     |
| article_prix_unitaire | Prix unitaire | double | W          |                                                     |
| article_prix_total    | Prix total    |        | R          | `::computePrice(article_qte,article_prix_unitaire)` |



Extrait du fichier de méthodes associé

    [php]
    public function computePrice($qte, $price){
        return intval($qte) * floatval($price);
    }

Note: on pourrait aussi utiliser une méthode statique, qui aurait sûrement plus de sens ici :

| attrid                | label         | type   | visibility | phpfunc                                                 |
| -                     |               |        |            |                                                         |
| article_qte           | Quantité      | int    | W          |                                                         |
| article_prix_unitaire | Prix unitaire | double | W          |                                                         |
| article_prix_total    | Prix total    |        | R          | `MathUtils::mult(article_qte,article_prix_unitaire)` |

    [php]
    class MathUtils {
        public static function mult($a, $b){
            return floatval($a) * floatval($b);
        }
    }

<!-- links -->
[WP_BNF]: http://fr.wikipedia.org/wiki/Backus_Naur_Form "Définition de la Forme de Backus-Naur sur Wikipedia"
[hooks]: #core-ref:8f3d47de-32b5-4748-8a00-b1569c5423e5
[refresh]:  #core-ref:0bab02de-50e0-46f2-8bdb-81c62dc86c93
[warning]:  #core-ref:75225e74-31cd-48cb-9297-a1cc412f1e75
