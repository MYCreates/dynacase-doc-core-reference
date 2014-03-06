# `[ACTION FDL:OPENDOC]` {#core-ref:f9e68fa7-01b7-4903-9718-744271d63112}

## Description  {#core-ref:1dfb7f56-d737-4b0b-8435-74892ffaebb5}

Cette action permet d'avoir la représentation HTML d'un document soit en édition,
soit en consultation.

Cette action appelle une autre action suivant le mode demandé :

* `edit` ou `new` : action GENERIC:GENERIC_EDIT,
* `view` : action FDL:FDL_CARD.

## Paramètres {#core-ref:e8e5c473-54b0-4eb0-8cd7-dc5b1cbc0238}

id
:   L'[id][id_document] du document source. Obligatoire si mode est `view` 
    ou `edit`.

mode
:   (`view`|`edit`|`new`). Si la valeur est `view` le document est affiché en 
    consultation. Si elle est `edit` ou `new` le document est affiché en édition.
    **obligatoire**.


## Paramètres : mode=view {#core-ref:0fb9377e-1117-473c-bd15-ce2519254820}

Dans ce mode l'application appelée est `FDL:FDL_CARD` et le document est affiché
en consultation.

latest
:   (`Y`|`L`|`P`) : Permet de spécifier quelle [révision][revise] du document
    est affichée :
    
    * `Y` : présente la dernière révision du document,
    * `L` : présente l'avant dernière révision du document,
    * `P` : présente la révision précédent celle de l'id,
    * tout autre valeur : présente la révision correspondant à l'id passé en paramètre.

state
:   Nom logique d'un état. Présente le dernier document de la lignée documentaire
    ayant cet état. Si `latest` et `state` sont présents, alors seul `state` est
    pris en compte.

zone
:   Nom d'une [zone documentaire][zoneDocumentaire] qui est appliquée au 
    document. Ce paramètre n'est pas pris en compte si le paramètre `vid` est valué.

vid
:   Nom d'une vue. Si un [contrôle de vue][cvdoc] est associé à ce document et
    que ce contrôle de vue contient une vue ayant ce nom, alors celle-ci est
    appliquée.

ulink
:   `N` : Si il est à `N` les liens ne sont pas rendus et ils sont remplacés par du
    texte sans hyperlien.

target
:    Contenu de l'attribut [target][MDNtarget] des liens générés.

inline
:   (`Y`|`N`) : Si la zone associée au document a l'option [`:B`][templateB], alors
    le fichier généré est envoyé avec un `Content-Disposition: inline`.
    (valeur par défaut : `N`)

unlock
:   (`Y`|`N`) : Si la valeur est à `Y` le document est déverrouillé avant sa
    consultation (défaut `N`).

dochead
:   (`N`) : Si la valeur est à `N` l'entête du document n'est pas affiché.


## Paramètres : mode=edit {#core-ref:6f77c5b1-c649-4835-962d-857960880757}

dirid
:   `int` : si c'est une création de document (`classid` ou `famid` et pas d'`id`)
    alors le document est créé dans le [dossier][dir] référencé par le `dirid`.

usefor
:   (`D`|`Q`) : Si usefor est à `D` alors c'est l'édition des valeurs par défaut
    qui est présentée. Si usefor est à `Q` alors c'est l'édition des paramètres
    qui est présentée. Ce paramètre n'est valide que dans le cas d'une création de 
    document.

mskid
:   Nom logique d'un masque. Si cet élément est valué alors ce masque est appliqué
    à l'interface d'édition.

zone
:   Nom d'une [zone documentaire][zoneDocumentaire] qui est appliquée au 
    document. Ce paramètre n'est pas pris en compte si le paramètre `vid` est valué.

vid
:   Nom d'une vue. Si un [contrôle de vue][cvdoc] est associé à ce document et
    que ce contrôle de vue contient une vue ayant ce nom, alors celle-ci est
    appliquée.

rzone
:   Nom d'une [zone documentaire][zoneDocumentaire] qui sera utilisée comme
    valeur de l'argument `zone` pour la page sur laquelle l'utilisateur est
    redirigé à la fin de l'édition du document.

rvid
:   Nom d'une vue qui sera utilisée comme valeur de l'argument `vid` pour la
    page sur laquelle l'utilisateur est redirigé à la fin de l'édition du
    document.

rtarget
:   Nom d'un fenêtre dans laquelle sera affichée la page sur laquelle
    l'utilisateur est redirigé à la fin de l'édition du document
    (valeur par défaut : `_self`).

autoclose
:   (yes). Si autoclose est à `yes` alors la fenêtre est fermée automatiquement
    après la modification.

## Paramètres : mode=new {#core-ref:00345042-c8a2-44a9-a351-43e646b09b0b}

Mode utilisé pour la création de document. Le paramètre `id` n'est pas pris en
compte.

Tous les paramètres du mode `edit` sont utilisables. 

Les paramètres spécifiques sont :

famid
:   Nom logique de la famille de document. Cet élément est utilisé pour créer
    un nouveau document. Si un `id` est fourni alors ce paramètre est ignoré.
    Obligatoire si `id` n'est pas fourni.

classid
:   Déprécié. Équivaut à `famid`. Si `classid` et `famid` sont fournis tous les 
    deux, alors seul `classid` est utilisé.

En plus de ces paramètres, il est possible d'utiliser les identifiants des
attributs comme variable HTTP pour pré-remplir des valeurs dans le formulaire.

    ?app=FDL&action=OPENDOC&mode=new&famid=1059&an_sexe=F&an_espece=1409

Cet exemple pré-rempli les attributs `an_sexe` et `an_espece`.

Pour les valeurs multivaluée, il faut utiliser la notation standard avec les
crochets.

    ?app=FDL&action=OPENDOC&mode=new&famid=1059&an_enfant[]=1433&an_enfant[]=1578

Cet exemple pré-rempli l'attribut `an_enfant` avec les deux valeurs 1433 et
1578.

Le pré-remplissage n'est pas pris en compte si l'attribut à une [zone d'édition
spécifique][editattrview].

**Important** : L'utilisation d'url avec des variables HTTP GET est limité en
longueur (variable suivant les configurations du serveurs). Il est déconseillé
d'utiliser la notation par URL GET si le nombre de variables et la longueur
n'est pas maîtrisé. Il est possible d'utiliser cette même URL avec des variables
POST pour contourner cette limite.

## Limites {#core-ref:96fc7d17-36bc-4dc6-bc90-9054fe380330}

N/A

## Notes {#core-ref:053a9043-0dfa-440f-a7fd-b1a684b8de05}

La page sur laquelle l'utilisateur est redirigé à la fin de l'édition du
document est : `app=FDL&action=FDL_CARD`.

<!-- link -->

[id_document]:          #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353 "Propriété ID"
[revise]:               #core-ref:882e3730-0483-4dbc-9b9d-0d0b5cc31d38
[zoneDocumentaire]:     #core-ref:49b96dc9-64e9-4f5a-a167-396282625c1e
[cvdoc]:                #core-ref:017f061a-7c12-42f8-aa9b-276cf706e7e0
[MDNtarget]:            https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a "Descriptif de la balise a"
[templateB]:            #core-ref:96d615e5-b6a6-46d3-b42d-4396dbc42b8b
[dir]:                  #core-ref:977910df-1dc1-4def-9e0b-fb938f5d849f
[editattrview]:         #core-ref:4faa4b17-56fc-4e42-a091-f1a97b7591b8