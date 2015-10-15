# Type `file` {#core-ref:0e904376-317c-426e-bc6d-e56fd52bad89}

## Description {#core-ref:75c33b8c-71c9-4086-be85-53da734d89bb}

Les attributs de type *file* permettent d'insérer un fichier.

## Représentation {#core-ref:5474b5b0-65fc-410c-8610-1bfe03f72a2b}

*   consultation :
    
    Un lien permettant de télécharger le fichier
    
    ![ file - consultation html ](famille/attributs/file-consultation.png "file - Consultation html")

*   modification :
    
    Un `input` de type `text` présentant le nom du fichier, suivi de 3 boutons :
    
    *   pour choisir un nouveau fichier,
    *   restaurer le fichier préalablement enregistré, 
    *   effacer le fichier.
    
    ![ file - Modification html ](famille/attributs/file-modification.png "file - Modification html")
    
    S'il s'agit d'une modification de document et que le fichier est déjà 
    enregistré alors le clic sur le nom du fichier permet de le télécharger.
    
    Des différences de comportement sont présentes en fonction des navigateur.
    Sur IE < 11, le bouton original "Parcourir" est affiché lorsqu'on clique sur le bouton "...".
    Le paramètre applicatif "FDL_OLDFILEINPUTCOMPAT" indique si l'attribut doit présenter,
    de manière systématique, le bouton original "Parcourir"
    
    ![ file - FDL_OLDFILEINPUTCOMPAT Modification html ](famille/attributs/file-old-modification.png "file - Compatibilité Modification html")

*   odt :
    
    Le titre du fichier
    
    ![ file - consultation odt ](famille/attributs/file-odt.png "file - Consultation odt")

## Comportement {#core-ref:b5195047-394d-496b-a25a-e6fad68fbcee}

Lors de l'enregistrement du document, le fichier est stocké dans le *vault*.

## Format de stockage {#core-ref:2ff4878c-6db2-419f-9dcc-64db69c769d2}

La valeur stockée est l'identifiant vault du fichier 
(sous la forme *`<type-mime>|<vaultid>|<file-name>`*).

Le type utilisé en base de donnée est `text`. 

## Options {#core-ref:cb7fd9e2-a5eb-4f4c-bff0-ed5593904ca5}

En plus des [options communes à tous les types d'attributs][commonopt], ce type
d'attribut dispose des options suivantes :

hideindav
:   Indique si le fichier apparaît lors de l'accès au moyen du protocole webdav.
    
    Les valeurs possibles sont :
    
    *   **`yes` (comportement par défaut)**
    *   `no`

inline
:   Si le paramètre est `yes`, il indique que le fichier doit être consulté 
    directement dans le navigateur.
    Seuls les types de fichiers supportés par le navigateur peuvent être affichés
    directement.  
    Si le paramètre est `no`, le téléchargement est proposé par le navigateur.
    Le nom du fichier proposé au téléchargement est celui du fichier enregistré.
    Si le nom du fichier comporte des double-quotes `"`, ils sont remplacés par
    des tirets `-`.
    
    Les valeurs possibles sont :
    
    *   `yes`
    *   **`no`(comportement par défaut)**

pdffile
:   Utilisé conjointement avec l'option *viewfiletype*, indique l'attribut contenant le fichier pdf à afficher.
    
    Les valeurs possibles sont :
    
    *   tout id d'attribut de la famille en cours.

preventfilechange
:   Ajoute une contrainte pour que le fichier à remplacer provienne de la dernière version du serveur.
    
    Cela ne bloque pas un changement de fichier mais avertit l'utilisateur dans le cas où le fichier ne correspond pas à cette dernière version.
    
    Lors du téléchargement du fichier un code identifiant la version est ajouté dans le nom du fichier (exemple foo{i47307-56}.ods pour le fichier foo.ods).
    Lorsque l'utilisateur envoie à nouveau le fichier, le serveur vérifie ce numéro de version. Si le numéro correspond à la dernière version, alors le fichier est accepté. Dans le cas contraire, une confirmation est demandée à l'utilisateur.
    
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

search
:   Indique si l'attribut est indexé pour la recherche plein-texte.
    
    Les valeurs possibles sont :
    
    *   **`yes` (comportement par défaut)**
    *   `no`

template
:   Indique si l'attribut file est un template.
    
    se reporter à la documentation des templates OOO pour les explications
    
    Les valeurs possibles sont :
    
    *   `static`
    *   `dynamic`

viewfileheigth
:   Utilisé conjointement avec l'option *viewfiletype*, indique la hauteur du rendu affiché sur le navigateur.
    
    Les valeurs possibles sont :
    
    *   Une taille en pixels (par exemple `150px`).
    *   Une taille en pourcentage de la hauteur de la fenêtre (par exemple `80%`)
    
    Il n'y a pas de valeur par défaut

viewfiletype
:   Indique qu'une prévisualisation du fichier sera disponible dans le navigateur, ne nécessitant donc pas de logiciel tiers.
    
    Les valeurs possibles sont :
    
    *   `pdf` : Dans ce cas, l'attribut indiqué par l'option *pdffile* sera utilisé comme prévisualisation ;
    *   `image` : Dans ce cas, le moteur de transformation génère une visualisation sous forme d'images avec tourne pages du pdf référencé par l'option *pdffile*.



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