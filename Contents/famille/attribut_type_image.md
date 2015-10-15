# Type `image` {#core-ref:4fca7712-59e0-4186-bfd0-6214104a0f60}

## Description {#core-ref:41e64bba-2ba9-4eb9-9768-f4df6f0bca43}

Les attributs de type *image* permettent d'insérer une image.

## Représentation {#core-ref:ce33d1ac-a7b6-4129-b8f2-ee5e11c02055}

*   consultation :
    
    une balise `img` alignée à droite.
    
    ![ image - consultation html ](famille/attributs/image-consultation.png "image - Consultation html")
    L'image affichée par défaut est une miniature de l'image 
    (sauf si l'option iwidth=auto) au format `png`.  
    <span class="flag inline from release">3.2.20</span> Dans le cas d'image 
    `jpeg`, la miniature est réorientée automatiquement 
    en fonction des données exif de l'image. Par contre, si c'est l'image 
    originale qui est affichée, elle n'est pas réorientée par le navigateur.  
    Les navigateurs (pour l'instant) ne tiennent pas compte de
    l'[orientation][setimageorient] dans les tags `image` mais seulement lors
    de l'affichage direct sur une page.


*   modification :
    
    Un `input` de type `text` présentant une miniature de l'image précédemment
         enregistrée et le nom du fichier, suivi de 3 boutons :
    
    *   pour choisir un nouveau fichier image,
    *   restaurer le fichier préalablement enregistré, 
    *   effacer le fichier image.
    
    ![ image - Modification html ](famille/attributs/image-modification.png "image - Modification html")
    
    Au survol de la miniature, une version plus grande est affichée.
    
    S'il s'agit d'une modification de document et que le fichier est déjà 
    enregistré alors le clic sur le nom du fichier permet de télécharger l'image.
    
    Des différences de comportement sont présentes en fonction des navigateur.
    Sur IE < 11, le bouton original "Parcourir" est affiché lorsqu'on clique sur le bouton "...".
    Le paramètre applicatif "FDL_OLDFILEINPUTCOMPAT" indique si l'attribut doit présenter,
    de manière systématique, le bouton original "Parcourir"
    
    ![ image - Modification html ](famille/attributs/image-old-modification.png "image - Compatibilité Modification html")

*   odt :
    
    Une image.
    
    ![ image - consultation odt ](famille/attributs/image-odt.png "image - Consultation odt")

## Comportement {#core-ref:bd47c52b-68a0-413c-8ed1-62e4f1f4b578}

Lors de l'upload du fichier, il est enregistré dans le *vault*.

## Format de stockage {#core-ref:69599309-970d-4b3c-a639-091cca7e1c21}

La valeur stockée est l’identifiant vault du fichier 
(sous la forme *&lt;type-mime&gt;|&lt;vaultid&gt;|&lt;file-title&gt;*).

Le type utilisé en base de donnée est `text`. 

## Options {#core-ref:b1a5a304-9eff-4e7a-8e0d-ab2363eaa346}

En plus des [options communes à tous les types d'attributs][commonopt], ce type d'attribut dispose des options suivantes :

hideindav
:   Indique si le fichier apparaît lors de l'accès au moyen du protocole webdav.
    
    Les valeurs possibles sont :
    
    *   **`yes` (comportement par défaut)**
    *   `no`

inline
:   Indique si l'image' doit être consultée directement dans le navigateur.
    Dans le cas contraire, le téléchargement est forcé.
    
    Les valeurs possibles sont :
    
    *   `yes`
    *   **`no`(comportement par défaut)**

iwidth
:   Indique la largeur de l'image dans l'interface web de consultation.
    
    L'image est redimensionnée coté serveur, avec respect des proportions.
    
    Les valeurs possibles sont:
    
    *   Une taille en pixels (par exemple `150px`).
    *   `auto` (dans ce cas, l'image est affichée dans sa taille originale).
    
    La valeur par défaut est `80px`.

preventfilechange
:   Ajoute une contrainte pour que le fichier à remplacer provienne de la dernière version du serveur.
    
    Cela ne bloque pas un changement de fichier mais avertit l'utilisateur dans le cas où le fichier ne correspond pas à cette dernière version.
    
    Lors du téléchargement du fichier un code identifiant la version est ajouté dans le nom du fichier (exemple foo{i47307-56}.ods pour le fichier foo.ods).
    Lorsque l'utilisateur uploade à nouveau le fichier, le serveur vérifie ce numéro de version.
    Si le numéro correspond à la dernière version, alors le fichier est accepté.
    Dans le cas contraire, une confirmation est demandée à l'utilisateur.
    
    Attention: si l'utilisateur renomme le fichier, la vérification échoue, et le serveur demande la confirmation.
    
    Les valeurs possibles sont :
    
    *   `yes`
    *   **`no` (comportement par défaut)**

rn
:   Indique que le fichier sera renommé sur le serveur.
    
    Les valeurs possibles sont :
    
    *   un nom de méthode de la famille courante (par exemple `::myNewName()`).
        La méthode prend en entrée le nom du fichier et doit retourner le nouveau nom sous la forme d'une chaîne de caractères.
    
    Note: Il est recommandé que la méthode fournisse une extension compatible avec le type mime pour l'utilisation ultérieure sur le poste client et les transformations.
    Pour récupérer l'extension d'un nom de fichier vous pouvez utiliser la fonction `getFileExtension` de la librairie `Lib.FileMime.php`



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
[commonopt]:        #core-ref:16e19c90-3233-11e2-a58f-6b135c3a2496
[setimageorient]:   https://developer.mozilla.org/en-US/docs/Web/CSS/image-orientation "MDN : css image-orientation"