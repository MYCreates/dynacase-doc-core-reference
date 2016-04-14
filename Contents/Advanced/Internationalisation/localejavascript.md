# Utiliser une traduction dans un programme JavaScript {#core-ref:0a8af47a-6a0b-4825-802a-308e4041729b}

Dynacase core ne fournit pas de fonction Javascript permettant de traduire du texte.
Cependant, il est possible d'utiliser la fonction fournie par le module _dynacase-datajs_,
ou d'utiliser les fonctions décrites ci-dessous

## Utilisation de _dynacase-datajs_ {#core-ref:91b45c4d-02ac-4958-8347-6f28c1b7b525}

Le module _dynacase-datajs_ fournit une bibliothèque JavaScript permettant
d'utiliser des traductions.

    [javascript]
    // Initialisation du contexte
    var C=new Fdl.Context({url:'http://www.mydomain.server/'});
    if (! C.isConnected()) {
        alert('error connect:'+C.getLastErrorMessage());
    return;
    }
    // Authentification
    if (! C.isAuthenticated()) {
        var u=C.setAuthentification({login:'mylogin',password:'secret'});
        if (!u)  alert('error authent:'+C.getLastErrorMessage());
    }
    // C is the context
    var helloText=C._("my test is ok");

La méthode `Fdl.Context::_()` retourne la traduction comme pour la fonction `_`
équivalente de PHP.

La langue choisie est celle de l'utilisateur (paramètre applicatif `CORE_LANG`).

<span class="flag inline nota-bene"></span> La récupération du catalogue est une opération JavaScript qui fait un 
appel Ajax **synchrone** au serveur, elle bloque donc l'exécution du JavaScript le 
temps de la récupération des traductions.

## Fonctions de traduction {#core-ref:b698d655-f021-41d3-9187-0703059bc7fc}

La fonction suivante peut être utilisée pour créer un _translator_ à partir d'un catalogue :

    [javascript]
    function translatorFactory(catalog)
    {
        
        var translation = {data: {catalog: {}}};
        try {
            translation = JSON.parse(catalog);
        } catch (e) {
            console.error("Locale catalog error : " + e.message);
        }
        return {
            /**
             * Return key translation
             * @param text to translate
             * @returns string
             */
            _: function translator_gettext(key)
            {
                if (key && translation.data && translation.data.catalog && translation.data.catalog[key]) {
                    return ranslation.data.catalog[key];
                }
                return key;
            },
            /**
             * Return key translation in context
             * @param text to translate
             * @param context
             * @returns {*}
             */
            ___: function translator_pgettext(key, ctxt)
            {
                if (key &&
                    translation.data &&
                    translation.data.catalog &&
                    translation.data.catalog._msgctxt_ &&
                    translation.data.catalog._msgctxt_[ctxt] &&
                    translation.data.catalog._msgctxt_[ctxt][key]) {
                    return translation.data.catalog._msgctxt_[ctxt][key];
                }
                return key;
            }
        };
    }

Le paramètre `catalog` correspond à un catalogue au format JSON.
Ce catalogue est [généré sur le serveur][core-ref:generation_catalogue] avec le path suivant :

    locale/<locale>/js/catalog.js

par exemple :

    locale/fr/js/catalog.js

Elle s'utilise de la manière suivante (le catalogue peut être récupéré autrement) :

    [javascript]
    var translator,
        translatedText;
    $.getJSON("locale/fr/js/catalog.js", function (catalog) {
        translator = getTranslator(catalog);
        translatedText = translator.___("key", "context");
    });

### Limitation {#core-ref:d9a3c4ac-aa89-43f4-8688-4e28759da201}

Les formes plurielles ne sont pas prises en compte.

<!-- links -->
[core-ref:generation_catalogue]:    #core-ref:2c163f00-8e94-4736-86f2-bb51352c52aa