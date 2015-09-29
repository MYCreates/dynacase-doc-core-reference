# Doc::postAffect()  {#core-ref:7e9f3b6f-f801-4fa9-8215-f02d575b357f}

<div class="short-description" markdown="1">  
<span class="flag from release inline">3.2.20</span>
[Hameçon][hook] (ou hook) utilisée par la fonction [`new_doc()`][newdoc] et pour
toute fonction qui récupère les données d'un document en base de données afin de
les affecter à l'objet comme par exemple lors de [recherche de
document][searchdoc].

</div>

## Description  {#core-ref:042e72a0-90bb-48e8-85bc-887af9e07075}

    [php]
    void protected function postAffect (array $data, 
                                        bool  $more , 
                                        bool  $reset )

Cette méthode permet de réaliser un traitement sur le document après affectation.
Au moment de l'appel, le document est initialisé avec les données founies par
l'argument "data".

### Avertissements  {#core-ref:84da462d-28aa-4aac-8848-a1cd840e7362}

Voir [avertissement `preAffect()`][warningpreaffect].

## Liste des paramètres  {#core-ref:325c3bdf-9c5c-451f-9456-5358aa6c781a}

(array) `data`

:   Contient les valeurs qui ont été affectées à l'objet. Le tableau est indexé par 
    les noms des attributs et propriétés du document.
    
    Les valeurs indexées ["values" et "attrids"][tabledoc] contiennent aussi
    l'ensemble des attributs sous une forme sérialisée.

(bool) `more`

:   Indique que si les données fournies par le paramètre `data` étaient incomplètes, 
    alors les données manquantes ont été récupérées depuis l'index "values" (s'il existe)
    fourni dans ce même paramètre `data`.
    
    *Note*: Dans le cas d'une recherche ce paramètre est mis à `true`

(bool) `reset`

:   Indique que toutes les données de la précédente affectation ont été effacées
    Cela concerne les propriétés et les attributs mais aussi les variables 
    privées.
    
    *Note*: Dans le cas d'une recherche ce paramètre est mis à `true`


## Valeur de retour  {#core-ref:66de5ef1-64ae-48c9-8cc8-c63b48264b7c}

Aucune.

## Erreurs / Exceptions  {#core-ref:d2b22083-a447-443a-a1c2-cb6189d2181e}

Aucune.

## Historique  {#core-ref:0a2ac9f6-d464-403b-9346-ef005aa07898}

Aucun.

## Exemples  {#core-ref:74f2417e-070c-4244-a60e-2d0eadd90c26}

Réinitialisation d'une variable privée en cas de réaffectation.

    [php]
    namespace My;
    use \Dcp\AttributeIdentifiers\MyFamily as MyAttributes;
    
    class MyFamily extends Dcp\Family\Document
    {
        const MYINIT=4;
        private $myPrivate=self::MYINIT;
        
        protected function postAffect(array $data, $more, $reset) {
            if ($reset) {
                $this->myPrivate=self::MYINIT;
            }
        }
    }

## Notes  {#core-ref:e6f8aeab-a1f1-4d9e-a444-4c50406c58ec}

Cette méthode n'est pas appelée si aucune donnée n'est fournie dans le tableau
`data`.

## Voir aussi  {#core-ref:d7ae353f-786b-4502-b361-b2509246fded}

*   [`Doc::preAffect()`][preaffect],

<!-- links -->

[warningpreaffect]:     #core-ref:308ebdfd-83cb-4db7-8b7e-20e466142580
[newdoc]:           #core-ref:e978cbd1-5f54-4a06-a6be-f1c079c2d734
[postaffect]:       #core-ref:7e9f3b6f-f801-4fa9-8215-f02d575b357f
[searchdoc]:        #core-ref:2367bd7b-d57a-4f8d-83df-2e92a7e38ed1
[notesearchdoc]:    #core-ref:867fcba6-94e7-403e-a523-73e20583a25f
[setobjectreturn]:  #core-ref:3a0b4882-81ff-4030-9f60-a0ed0ff1f958
[hook]:             http://fr.wikipedia.org/wiki/Hook_(informatique) "Définition de Hook sur wikipedia"
[tabledoc]:         #core-ref:d4b8d8ce-6f7a-4c1c-a5c4-f1adfcb74864
[preaffect]:        #core-ref:e6f36fea-9f42-4751-ba9b-c3aafec56559