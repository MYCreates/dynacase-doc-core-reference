# Encodage pour format HTML et XML {#core-ref:751e00d9-eec7-41b7-835f-14df8c0b7821}

## Introduction

Lors de la génération d'une page HTML ou XML contenant des données externes,
il est important d'encoder correctement les données externes de tel manière
que celles-ci ne soient pas considérée comme une partie du XML ou du HTML mais
bien comme du texte.

Le moteur de template de Dynacase met à disposition deux méthodes permettant
d'encoder du texte : [Layout::eSet()][eSet],
[Layout::eSetBlockData()][eSetBlockData].

Ces méthodes permettent d'encoder les chaînes de caractères intégrées dans un
template pour que celles-ci ne soient pas considérées comme du XML valide mais
comme du texte.

On peut donc les considérer comme une protection contre les failles de types 
[XSS][XSS].

## Avertissements

Les méthodes [Layout::eSet()][eSet], [Layout::eSetBlockData()][eSetBlockData] ne
doivent être utilisées que dans le cas d'affectation d'une variable dont on
sait que le contenu est du texte. Si le contenu attendu est du HTML ou XML, il
faut utiliser [set][set].

Dans le cas de la production de XML, il faut penser à [ajouter][inclusion] les 
[entités][entite] nécessaires.

De plus, les `&` dans les URL doivent être encodés en `&amp;`

## Exemple

Soit une action permettant de montrer à un utilisateur le titre de son 
animal préféré, le code est le suivant :

Code PHP :

    [PHP]
    require_once "FDL/freedom_util.php";
    
    function zoo_favorite(Action & $action)
    {
    
        $doc = \Dcp\DocManager::getDocument( "FAVORITE");
        $action->lay->set("TITRE", $doc->getTitle());
    
    }

Layout :

    [html]
    <!doctype html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        Bonjour !
        
        Ceci est le titre de votre document animal préféré : <strong>[TITRE]</strong>.
    </body>
    </html>

Si le nom du document animal favoris est  `Clovis`.

La page suivante est obtenue :

    [html]
    <!doctype html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        Bonjour !
        
        Ceci est le titre de votre document animal préféré : <strong>Clovis</strong>.
    </body>
    </html>

Maintenant, si le nom est  : `<a>Mon Préféré`

La page suivante est obtenue :

    [html]
    <!doctype html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        Bonjour !
        
        Ceci est le titre de votre document animal préféré : <strong><a>Mon Préféré</strong>.
    </body>
    </html>

Ce qui donne :

![ Html mal encodé ](advanced/template/encode_bad.png)

Cette page a deux défauts :

* lorsqu'elle est rendu, on ne voit plus le `<a>` car il a été interprété comme
du HTML,
* ce n'est pas du HTML valide car elle contient une balise `<a>` mais pas de 
balise fermante `</a>`.

Nous allons faire évoluer notre code PHP, en utilisant au lieu de
[Layout::set()][set] la méthode [Layout::eSet()][eSet].

On a donc le PHP suivant :

    [php]
    require_once "FDL/freedom_util.php";
    
    function zoo_favorite(Action & $action)
    {
    
        $doc = \Dcp\DocManager::getDocument( "FAVORITE");
        $action->lay->eSet("TITRE", $doc->getTitle());
    
    }

Et on génère le HTML suivant :

    [html]
    <!doctype html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>
    <body>
        Bonjour !
        
        Ceci est le titre de votre document animal préféré : <strong>&lt;a&gt;Mon Préféré</strong>.
    </body>
    </html>

Ce qui donne :

![ Html bien encodé ](advanced/template/encode_good.png)

La partie `<a>` a été encodée et apparaît dans le HTML comme `&lt;a&gt;` et est
rendue sous la forme suivante `<a>`.


[set]:  #core-ref:812c30ed-11cb-4b59-84d2-ba10e4ab7e88
[eSet]: #core-ref:2696710a-f491-4887-b953-e08d918ef4fb
[eSetBlockData]: #core-ref:088e711c-ea91-45e7-841d-289ffc53c80b
[XSS]:  https://fr.wikipedia.org/wiki/XSS "Wikipédia XSS"
[entite]: #core-ref:2c99cfbe-e12c-44a7-a23a-3a6bd8e106d7 "Entités XML"
[inclusion]: https://fr.wikipedia.org/wiki/Xml#Inclusions "Wikipédia : XML"