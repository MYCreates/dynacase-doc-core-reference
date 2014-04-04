# Recherche de document depuis leur identifiant {#core-ref:de554832-30f5-485a-8303-31cf464d613d}

Si vous disposez d'une liste d'identifiant de document, il n'est pas conseillé
de réaliser une boucle avec l'appel à la méthode
[`DocManager::getDocument()`][getdocument] car cela se révèle lent et
consommateur en mémoire. Il est conseillé dans ce cas d'utiliser l'itérateur de
document.

    [php]
    $dl=new DocumentList();
    $dl->addDocumentIdentifiers(array(1112, 1110, 1120, 2034));
    foreach ($dl as $fam) {
        print $fam->getTitle()."\n";
    }

La méthode `DocumentList::addDocumentIdentifiers()` permet de renseigner la
liste des identifiants de document.

Si dans la liste un ou plusieurs identifiant n'existe pas alors l'itérateur ne
les retourne pas. Dans ce cas le nombre de documents retourné est inférieur au
nombre d'identifiants donnés.

Par défaut seuls les document que l'utilisateur a le droit de voir sont
retournés.

Si vous voulez affiner les critères de recherche vous pouvez le faire en
utilisant la recherche stockée dans l'iterateur.

    [php]
    $dl=new DocumentList();
    $dl->addDocumentIdentifiers(array(1180, 3852, 3853));
    $dl->getSearchDocument()->addFilter("title ~ 'Doe'");
    $dl->getSearchDocument()->overrideViewControl();

Cela ne retournera que les documents dont le titre contient *Doe* parmi les
trois documents donnés sans tenir compte des droits de visibilités. Il est aussi
possible d'utiliser la fonction de callback pour l'appliquer à l'ensemble
de la liste (voir `DocumentList::listMap()`).


<!-- link -->
[searchdoc]:        #core-ref:a5216d5c-4e0f-4e3c-9553-7cbfda6b3255
[propdoc]:          #core-ref:9aa8edfa-2f2a-11e2-aaec-838a12b40353 "Liste des propriétés du document"
[layoutblock]:      #core-ref:587b563e-7371-469f-9d1e-350607056c73
[formatcollection]: #core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81
[pgop]:             http://www.postgresql.org/docs/9.1/static/functions.html "Opérateurs Postgresql 9.1"
[docattributs]:     #core-ref:4e167170-33ed-11e2-8134-a7f43955d6f3
[attdocid]:         #core-ref:d461d5f5-b635-47a0-944d-473c227587ab
[phpiterator]:      http://php.net/manual/fr/class.iterator.php "Interface Iterator"
[docacl]:           #core-ref:a99dcc5f-f42f-4574-bbfa-d7bb0573c95d "Droits du document"
[getdocument]:      #core-ref:dfa0762f-6ff3-4349-bd21-6442740d9dcc