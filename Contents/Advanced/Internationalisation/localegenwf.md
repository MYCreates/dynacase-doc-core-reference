# Extraction des clés pour les workflows {#core-ref:521dc8f1-287e-4a6e-85c4-e5698421146f}

Afin que les clés de traduction soient présentes dans le catalogue de traduction,
il est nécessaire de les identifier.
Une des solutions consiste à ajouter une fonction factice qui sert
à identifier les éléments à traduire.

## Exemple {#core-ref:ae8b9cfd-0a43-4d56-a8ad-12fe5a3aa0b1}

    [php]
    namespace MyTest;
    
    class WflTest extends \Dcp\Family\WDoc
    {
        public $attrPrefix = "TST";
        public $firstState = self::etat_created;
        
        //region States
        const etat_created    = "myapp_tst_e1";
        const etat_redacted   = "myapp_tst_e2";
        //endregion
        
        //region Transitions
        const transition_created_redacted    = "myapp_tst_t1";
        const transition_redacted_created    = "myapp_tst_t2";
        //endregion
        
        // Activities
        public $stateactivity = array(
            self::etat_created    => "tst:activity:etat_created",
            self::etat_redacted   => "tst:activity:etat_redacted",
        );

Les clefs des états et des transitions sont leurs identifiants. Il est
nécessaire d'identifier ces clefs de traduction pour le programme de génération
de catalogue. 

    [php]
    private function i18n () {
        $i18n=_("myapp_tst_e1"); // states
        $i18n=_("myapp_tst_e2");
        $i18n=_("myapp_tst_t1"); // transitions
        $i18n=_("myapp_tst_t2");
        $i18n=_("tst:activity:etat_created"); // activities
        $i18n=_("tst:activity:etat_redacted");
    }

Cette fonction peut être soit une fonction privée du cycle de vie, soit être
présente dans un autre fichier qui n'est pas déployé.