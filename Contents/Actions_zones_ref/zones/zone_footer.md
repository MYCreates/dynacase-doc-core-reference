# Paramétrage du pied de documents {#core-ref:bc027994-70fd-4c17-9f55-577a21f717e6}

Une représentation standard HTML du document possède trois parties : 

1. une entête (titre et menu)
2. le contenu (attributs affichés)
3. un pied de document (footer) - vide par défaut

Le pied de document (_document footer_) est personnalisable en ajoutant
une ou plusieurs [zone][zones].

La méthode :

    DocumentUserInterface::addDocumentFooterZone(string $index, 
                                                 string $zone, 
                                                 bool   $edit)

permet d'ajouter ou de remplacer une zone dans le pied de document. Ce
paramétrage est effectif pour les [interfaces HTML standards][opendoc] de
consultation et de modification du document fournies par Dynacase Core.

Les paramètres de cette méthode sont :

1.   Une chaîne de caractères représentant un nom de référence pour la zone.
2.   Une chaîne de caractères représentant la définition de la [zone][zonedef] 
3.   Un booléen permettant de savoir si la zone déclarer doit être utilisée en mode 
     de consultation (paramètre à `false`) ou en mode d'édition : (paramètre à `true`).

Cette initialisation peut être faite via le [fichier de configuration][appinit]
de l'application ou via un script [wsh][wsh].

## Exemple de déclaration de pied de document {#core-ref:5386a4db-ff97-497c-95f4-3d250cc3c746}

Fichier MONAPPLICATION_init.php.in

    [php]
    $app_const = array(
    "INIT" => "yes",
    "VERSION" => "@VERSION@-@RELEASE@"
    );
    //Visible en mode de consultation sur tous les documents
    DocumentUserInterface::addDocumentFooterZone(
        "MYCUSTOMFOOT",
        "MONAPPLICATION:ZONE_MONAPPLICATION?id=[id]", 
        false); // View Document Footer 
    //Visible en mode d'édition sur tous les documents.
    DocumentUserInterface::addDocumentFooterZone(
        "MYCUSTOMFOOT",
        "MONAPPLICATION:ZONE_MONAPPLICATION?id=[id]&type=form",
        true); // Form Document Footer 
    

Cela ajoute la zone de l'application MONAPPLICATION, s'appelant
ZONE_MONAPPLICATION dans le pied de document sur l'interface de consultation et
de modification.

Des [paramètres][zonedef] peuvent être ajoutés pour la zone afin de rendre le
contrôleur plus générique.

L'identifiant du document est indiqué avec la variable : 

*   `[id]`: L'identifiant du document

Une fois les zones ainsi déclarées, elles seront disponibles, et affichées sur
tous les documents. Le contrôleur de la zone peut rendre conditionnel
l'affichage en fonction des arguments qui lui sont fournis.

Si plusieurs zones sont déclarées, elles sont générées les unes à la suite des
autres dans l'ordre de leurs déclarations.

## Exemple pied de document {#core-ref:a0e20794-27f9-438b-8f3e-e2ad60996b3e}

Cet exemple montre comment réaliser un exemple complet de pied de document avec
le contrôleur, le template ainsi que l'ajout d'une feuille de style et de
javascript.

Fichier : MONAPPLICATION/zone_application.php (le contrôleur)

    [php]
    function zone_monapplication(Action &$action) {
        $docid=$action->getArgument("id");
        $type=$action->getArgument("type", "view");
        
        $action->parent->AddCssRef("MONAPPLICATION/Layout/zone_monapplication.css");
        $action->parent->AddJsRef("MONAPPLICATION/Layout/zone_monapplication.js");
        
        if ($type === "form") {
            $action->lay->eset("MYKEY", "Formulaire à remplir");
        } else {
            $action->lay->set("MYKEY", "&copy; My company");
        }
    }

Fichier : MONAPPLICATION/Layout/zone_application.xml (le template)

    [html]
    <footer class="myFoot">
      [MYKEY]
    </footer>


Fichier : MONAPPLICATION/Layout/zone_application.css (la feuille de style)

    [css]
    footer.myFoot  {
        background : yellow;
        text-align:center;
    }


Fichier : MONAPPLICATION/Layout/zone_application.js (le code javascript)

    [js]
    console.log("I'm here");

La définition d'un pied de document peut aussi être construite avec un template
vide afin de réaliser une simple opération d'ajout de _css_ ou de _javascript_.



<!-- links -->

[zone]:     #core-ref:2cd69ba4-601c-407d-ad5c-fc917bbbce02
[zonedef]:  #core-ref:28f377e2-b9ee-4427-b2ef-7134eed2bf72
[wsh]:      #core-ref:4df1314f-9fdd-4a7f-af37-a18cc39f3505
[appinit]:   #core-ref:64c24cf9-b646-4a0f-9164-d0d146e12023
[opendoc]:  #core-ref:f9e68fa7-01b7-4903-9718-744271d63112