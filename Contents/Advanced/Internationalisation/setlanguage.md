# Modifier la langue dans un programme {#core-ref:1d2f5742-4171-11e3-9773-cffb8e583c34}

Par défaut, le catalogue correspondant à la locale de l'utilisateur connecté est
chargé. Il est possible de changer le catalogue en cours d'exécution en
utilisant la fonction `setLanguage`.

    [php]
    setLanguage('fr_FR');
    print ___("Hello", "tstCtx");
    // => Bonjour à tous
    setLanguage('en_US');
    print ___("Hello", "tstCtx");
    // => Hello everybody

Le paramètre doit être la locale complète sur cinq lettres.
