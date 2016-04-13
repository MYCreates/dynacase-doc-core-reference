# Type `menu` {#core-ref:c976efc3-dc70-463e-a147-2934c96b7bb3}

## Description {#core-ref:eaa7c448-f7aa-4ba6-9a90-9c284e071962}

Les attributs de type *menu* se présentent sous la forme de liens présent dans
la barre de menu en haut du document.

Ces menus permettent d'accéder à des urls, et ainsi de déclencher des actions,
etc.

L'url du menu est indiqué dans la [caractéristique `link`][alink].

Cette caractéristique supporte, en plus des notations standards, la notation :

    ::myMethod()

Cette notation permet d'appeler directement une méthode exposée de la famille.
L'appel de cette méthode retourne la page de consultation de document mis à jour.

La méthode appelée doit avoit le tag `@apiExpose` dans le commentaire associée
afin de la rendre exposable. Cette méthode doit assurer elle-même les contrôles 
nécessaires à la sécurité du document.

## Représentation {#core-ref:f6520991-b990-4305-af74-80e8a6dd2341}

*   consultation :
    
    Une entrée de menu supplémentaire.
    
    ![ menu - consultation html ](famille/attributs/menu-consultation.png "menu - Consultation html")

*   modification :
    
    Aucune représentation.

*   odt :
    
    Aucune représentation.

## Comportement {#core-ref:7d8fed7b-0e07-4a54-ae02-69345de63a74}

Le menu n'est pas représenté dans les cas suivants:

*   *link* est vide,
*   la visibilité est `H`,
*   le menu référence des attributs au moyen de la syntaxe *&lt;attrid&gt;* 
et un de ces attributs est vide (cela peut être contourné par l'utilisation 
de la syntaxe ?&lt;attrid&gt;).

## Format de stockage {#core-ref:0540a07e-5b71-4591-bbcd-4a048d6e3cac}

Cet attribut n'est pas stocké.

## Visibilité {#core-ref:0abcbf60-b1c5-4d78-b3a7-6574a369d99e}

Le menu est soumis à la [visibilité][visibility] standard. 
Si la visibilité est `H`, ou si l'url est vide le menu n'est pas affiché.

Si le champ [phpfunc][phpfunc] indique une méthode, alors cette méthode sera utilisé 
pour avoir la visibilité du menu. Cette méthode outrepasse la visibilité indiqué
dans l'attribut ou le [masque][mask].
La méthode doit retourner une des constantes suivantes :

*   `MENU_ACTIVE` : Le menu est visible et actif (cliquable)
*   `MENU_INVISIBLE` : Le menu n'est pas visible
*   `MENU_INACTIVE` : Le menu est visible mais il est inhibé (grisé)

Exemple :

Utilisation de la fonction `::menuResetLoginFailure()` déclarée dans la colonne
`phpfunc` :

    [php]
    class myFamily extends \Dcp\Family\Document
        public function menuResetLoginFailure()
        {
            // Do not show the menu if the user has no edit rights on the document
            if ($this->canEdit() != '') {
                return MENU_INACTIVE;
            }
            // Do not show the menu on the 'admin' user
            if ($this->getRawValue('us_whatid') == 1) {
                return MENU_INVISIBLE;
            }
            // Do not show the menu if the account has no failures
            if ($this->getRawValue("us_loginfailure") <= 0) {
                return MENU_INVISIBLE;
            }
            return MENU_ACTIVE;
        }
    }

## Options {#core-ref:53f09af0-7ca1-4a3b-b3a1-1fc4455d371d}

En plus des [options communes à tous les types d'attributs][commonopt], ce type
d'attribut dispose des options suivantes :

barmenu
:   Indique que la popup doit s'ouvrir avec la barre de menus du navigateur.
    
    Les valeurs possibles sont :
    
    *   `yes`
    *   **`no` (comportement par défaut)**

global
:   Indique que l'action effectuée par le menu n'est pas liée à un document en particulier.
    
    Dans ce cas, le menu apparaît aussi dans le menu 'outils' des applications
    issues de GENERIC, ainsi que dans le menu contextuel du document famille.
    
    Les valeurs possibles sont :
    
    *   `yes`
    *   **`no` (comportement par défaut)**

lconfirm
:   Indique qu'une confirmation doit être demandée avant activation du lien.
    
    Le texte de la confirmation est déterminé par l'option *tconfirm*.
    
    Les valeurs possibles sont :
    
    *   `yes`
    *   **`no` (comportement par défaut)**

lcontrol
:   Indique que le menu est disponible uniquement dans le menu contextuel du document,
    lorsque la touche *control* est appuyée.
    
    Les valeurs possibles sont :
    
    *   `yes`
    *   **`no` (comportement par défaut)**

mheight
:   Indique la hauteur de la fenêtre popup.
    
    Les valeurs possibles sont :
    
    *   toute hauteur en pixels (par exemple `400px`)
    
    La valeur par défaut est **`300px`**

mtarget
:   Indique le *name* de la fenêtre cible de l'hyperlien.
    
    Les valeurs possibles sont :
    
    *   **`_blank` (comportement par défaut)** : Dans ce cas, la fenêtre cible est une nouvelle fenêtre ;
    *   `_self` : Dans ce cas, la fenêtre cible est la fenêtre en cours ;
    *   `fhidden` : Dans ce cas, la fenêtre cible est une fenêtre cachée ;
    *   `fdoc` : Dans l'application *ONEFAM*, la fenêtre cible est la fenêtre dans laquelle est affiché le document (utile pour les menus avec l'option *global*) ;
    *   tout nom de fenêtre valide.

mwidth
:   Indique la largeur de la fenêtre popup.
    
    Les valeurs possibles sont :
    
    *   toute largeur en pixels (par exemple `400px`)
    
    La valeur par défaut est **`400px`**

onlyglobal
:   Utilisé conjointement avec l'option *global*, indique que le menu ne doit pas apparaître sur le document.
    
    Les valeurs possibles sont :
    
    *   `yes`
    *   **`no` (comportement par défaut)**

submenu
:   Indique que le menu doit avoir un menu parent.
    
    Les valeurs possibles sont :
    
    *   toute chaîne alphanumérique.

tconfirm
:   Indique la question apparaissant pour la confirmation (ce doit être une question fermée).
    
    Cette question est automatiquement ajoutée au catalogue de traduction.
    
    Les valeurs possibles sont :
    
    *   toute chaîne de caractères.
    
    La valeur par défaut est **`Êtes-vous sûr ?`**



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
[alink]:        #core-ref:aaaa5d78-0982-4c3e-a8ed-a125c49572a8
[commonopt]:    #core-ref:16e19c90-3233-11e2-a58f-6b135c3a2496
[constraint]:   #core-ref:f0177c62-1774-4724-a337-f090406e2d08
[visibility]:   #core-ref:3e67d45e-1fed-446d-82b5-ba941addc7e8
[mask]:         #core-ref:327ad491-06df-4e5b-b49a-695c75439fe1
[phpfunc]:      #core-ref:1128e658-48f5-440f-9fd1-2d714e99eecd