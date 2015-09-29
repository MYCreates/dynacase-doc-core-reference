# Doc::preAffect()  {#core-ref:e6f36fea-9f42-4751-ba9b-c3aafec56559}

<div class="short-description" markdown="1">  
<span class="flag from release inline">3.2.20</span>
[Hameçon][hook] (ou hook) utilisée par la fonction [`new_doc()`][newdoc] et pour
toute fonction qui récupère les données d'un document en base de données afin de
les affecter à l'objet comme par exemple lors de [recherche de
document][searchdoc].

</div>

## Description  {#core-ref:e727dea8-a4fa-41dc-8970-1c946cfe8779}

    [php]
    void protected function preAffect (array &$data, 
                                        bool &$more , 
                                        bool &$reset )

Cette permet de contrôler l'affectation des données avant leur affectation dans
l'objet. Au moment de l'appel, l'objet document contient les valeurs affectées
du précédent document.

Cette méthode ne peut pas empêcher l'affectation mais peux modifier les données
affectées. 


### Avertissements  {#core-ref:308ebdfd-83cb-4db7-8b7e-20e466142580}

Cette méthode est appelée pour toute affectation d'objet documentaire. Elle est
appelée principalement par les fonctions de récupération de document et de
recherche mais aussi par les méthode de révision et de changement d'état.

Cette méthode étant *bas-niveau*, les tests et traitement définis dans cette
fonction peuvent pénaliser les temps de traitement.

Lors d'une recherche, cette méthode est appelée pour chacun des document
trouvés si l'option de [retour][setobjectreturn] précise le retour d'objet. Dans
le cas de retour brut, cette méthode n'est pas appelée car il n'y a pas de
création d'objet.

## Liste des paramètres  {#core-ref:88d6a586-ad8a-4405-b019-89d1379134b8}

(array) `data`

:   Contient les valeurs qui seront affectées à l'objet. Le tableau est indexé par 
    les noms des attributs et propriétés du document.
    
    Ces valeurs peuvent être modifiées. Le tableau "data" sert ensuite lors de
    l'affectation.
    
    Les valeurs indexées ["values" et "attrids"][tabledoc] contiennent aussi
    l'ensemble des attributs sous une forme sérialisée.

(bool) `more`

:   Indique que si les données fournies par le paramètre `data` sont incomplètes, 
    alors les données manquantes sont récupérées depuis l'index "values" (s'il existe)
    fourni dans ce même paramètre `data`.  
    Il est possible de modifier ce paramètre afin de forcer ou inhiber ce fonctionnement.  
    
    *Note*: Dans le cas d'une recherche ce paramètre est mis à `true`

(bool) `reset`

:   Indique que toutes les données de la précédente affectation seront effacées
    Cela concerne les propriétés et les attributs mais aussi les variables privées.
    Il est possible de modifier ce paramètre afin de forcer ou inhiber le `reset`.
    
    *Note*: Dans le cas d'une recherche ce paramètre est mis à `true`


## Valeur de retour  {#core-ref:87e4afcf-ee42-4498-a52f-3827f5dd9cfa}

Aucune.

## Erreurs / Exceptions  {#core-ref:2f503bf6-b646-464a-b36b-3732d80f2d85}

Aucune.

## Historique  {#core-ref:3bd685c6-7d9a-4c18-91ce-4ef9730c4742}

Aucun.

## Exemples  {#core-ref:ac2b7315-15b4-424d-b735-502c26225f0f}

Forcer l'affectation d'un attribut suivant une certaine condition. Si aucun
message d'avertissement (`my_warning`) n'est indiqué et le niveau (`my_order`)
est supérieur à 10 alors on indique un message par défaut.

    [php]
    namespace My;
    use \Dcp\AttributeIdentifiers\MyFamily as MyAttributes;
    
    class MyFamily extends Dcp\Family\Document
    {
        protected function preAffect(array &$data, &$more, &$reset)  {
            if (!$data[MyAttributes::my_warning]) {
                if ($data[MyAttributes::my_order] > 20 ) {
                    $data[MyAttributes::my_warning]="Emergency level priority";
                } elseif ($data[MyAttributes::my_order] > 10 ) {
                    $data[MyAttributes::my_warning]="High level priority";
                } 
            }
        }
    }

En cas d'affichage de [rapport][report] ou de document, l'attribut `my_warning`
sera affiché suivant l'ordre `my_order`. Par contre, cela ne constitue pas une
affectation en base de donnée sauf si l'affectation est suivie d'une
[sauvegarde][store].


## Notes  {#core-ref:9db56c38-9f7a-4db0-ae52-30d958821984}

Cette méthode n'est pas appelée si aucune donnée n'est fournie dans le tableau
`data`.

## Voir aussi  {#core-ref:83aaf555-0514-4b01-bcf4-c22ad5ee804c}

*   [`Doc::postAffect()`][postaffect],
*   [Note searchDoc][notesearchdoc]

<!-- links -->
[newdoc]:           #core-ref:e978cbd1-5f54-4a06-a6be-f1c079c2d734
[postaffect]:       #core-ref:7e9f3b6f-f801-4fa9-8215-f02d575b357f
[searchdoc]:        #core-ref:2367bd7b-d57a-4f8d-83df-2e92a7e38ed1
[notesearchdoc]:    #core-ref:867fcba6-94e7-403e-a523-73e20583a25f
[setobjectreturn]:  #core-ref:3a0b4882-81ff-4030-9f60-a0ed0ff1f958
[hook]:             http://fr.wikipedia.org/wiki/Hook_(informatique) "Définition de Hook sur wikipedia"
[tabledoc]:         #core-ref:d4b8d8ce-6f7a-4c1c-a5c4-f1adfcb74864
[store]:             #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[report]:           #core-ref:32bc0b13-2b9f-4096-ac83-15f2b40d3b39