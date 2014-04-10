# `[ACTION FDL:EXPORTFILE]` {#core-ref:0fc93676-888f-497e-b9bc-28cdbead52df}

## Description  {#core-ref:05ce2a2b-f1a9-4c01-b5e5-89f33134781a}

Cette action permet de télécharger un fichier du [coffre][dbvault]. Soit à l'aide
de son identifiant vault, soit de sa référence dans un document.

## Paramètres {#core-ref:58aa4bf4-21fb-40ac-919e-fa4650d4d0fb}

inline
:   `yes`. Si la valeur est `yes` le fichier est servi inline (affiché dans la
    page) sinon il est télécharger. (valeur par défaut : vide).

cache
:   `yes`. Si la valeur est `yes` le fichier est téléchargé du cache s'il est 
    présent en cache. (valeur par défaut : `yes`).


width
:   `int` : Si le mime type du fichier est de type image ou que le type est `png`
    alors dans ce cas le fichier est redimensionné avec la largeur en pixel de
    width tout en respectant son ratio.

cvViewid <span class="flag from release">Release 3.2.12</span>
:   Identifiant de la vue à utiliser en cas de contrôle de vue. Nécessaire
    seulement si l'attribut fichier à la [visibilité][visibility] `I` par défaut.

## Paramètres `docid` {#core-ref:21b65043-d0ee-47f0-8956-caff43d4d74a}

docid
:   L'[identificateur][id_document] du document source.

id
:   Équivalent de l'argument `docid`, il n'est pas pris en compte si `docid` est
    renseigné.

latest
:   (Y|L|P) : Indique quelle [révision][revise] du document est affichée :
    
    * Y : présente la dernière révision du document,
    * L : présente l'avant dernière révision du document,
    * P : présente la révision précédent celle de l'id,
    * tout autre valeur : présente la révision correspondant à l'id passé en paramètre.

state
:   Identifiant d'un état. Présente le dernier document de la lignée documentaire
    ayant cet état. Si latest et state sont présents alors seul state est pris
    en compte.

attrid
:   L' [attrid][property] de type [`file`][fileattr] ou [`image`][imgattr] du 
    document que l'on souhaite présenter.

index
:   `int` : Dans le cas d'un attrid référençant un attribut multiple. L'index 
    permet de n'exporter que le n-ième élément. La numérotation commence à 0.

## Paramètre pour fichier avec conversion PDF {#core-ref:30de106b-5b3e-4104-9ded-6b5e5e8d0f8a}

Ces options ne sont applicables que si l'attribut possède un fichier `pdf`
calculé par [le moteur de transformation][te]. En standard, le moteur de
transformation peut convertir les fichiers bureautique les plus courants (doc,
docx, xls, xlsx, ppt, ...).
 
type 
:    (pdf|png) si le format est `png` le `pdf` calculé est ensuite transformé 
    en `png`.
    si le format est `pdf`, c'est le fichier `pdf` calculé qui est retourné.

page
:   `int` : Si le type est `png` alors cet attribut est obligatoire, il indique
    la page du png demandé.

## Paramètres `vaultid` {#core-ref:56a0f17e-7de9-4dd5-9ad1-e4e5a966c618}

Pour récupérer un fichier à l'aide de son [identifiant vault][vaultid]. 

Ceci n'est applicable que si le fichier a été enregistré dans le coffre **en
accès libre**.

**Attention** : Si un identifiant `vaultid` est fourni, les paramètres liés aux 
[docid][docid] ne sont pas pris en compte.

vaultid
:   [identifiant vault][vaultid] du fichier à télécharger

## Limites {#core-ref:81911403-f03e-4071-9d17-12db912467e5}

N/A

<!-- link -->

[id_document]:      #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353 "Propriété ID"
[vaultid]:          #core-ref:2ff4878c-6db2-419f-9dcc-64db69c769d2
[docid]:            #core-ref:21b65043-d0ee-47f0-8956-caff43d4d74a
[property]:         #core-ref:bc3fad86-33cc-11e2-9a69-1bbd9c32b0f2
[te]:               https://docs.anakeen.com/dynacase/3.2/dynacase-doc-tengine-installation-operating/website/book/index.html "Moteur de transformation"
[dbvault]:          #core-ref:7c41b8d8-5ace-489c-886f-a6500c717423
[fileattr]:         #core-ref:0e904376-317c-426e-bc6d-e56fd52bad89
[imgattr]:          #core-ref:4fca7712-59e0-4186-bfd0-6214104a0f60
[visibility]:        #core-ref:3e67d45e-1fed-446d-82b5-ba941addc7e8