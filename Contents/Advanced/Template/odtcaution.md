# Utilisation du contrôleur pour template ODT {#core-ref:84399e7b-57a8-4ac9-a675-d50818e294bc}
## Précautions d'usage {#core-ref:2705b0cf-b697-4785-a961-b2ee4d7f42e0}

Les templates ODT manipulent des données XML. La syntaxe des template XML est
plus rigoureuse que les template HTML. Les fichiers générés peuvent être
invalides si les données insérées ne sont pas conformes aux contraintes imposées
par le format XML.

Voir la liste des [limitations des ODT][odt_limitation].

## Affectation de variables {#core-ref:728e85c3-e88b-4817-9d77-a95ed103c543}

La classe PHP de manipulation de template est la classe `OooLayout` qui dérivé de la
[classe `Layout`][classlayout].

Les précautions d'usages sont les suivantes :

* lors de l'affectation d'une variable, il ne faut pas que celle-ci corrompe le 
XML pour ce faire, on peut : 
    * utiliser la fonction d'affectation dédiée 
        `$this->lay->eSet("VAR", $var);`, 
    * encoder par avance la valeur avant de l'insérer 
        
            [php]
            $society = $this->lay->xmlEntities("Bob & Compagnie") ;
            $this->lay->set("SOCIETY",$society) ;
    
    * ou encore si la valeur provient d'un document :
        
            [php]
            $myAnimal = \Dcp\DocManager::getDocument( "MY_ANIMAL");
            $name = $myAnimal->getValue("animal_name");
            $name = $myAnimal->getOooValue($myAnimal->getAttribute("the_name"), $name);
            $this->lay->set("MY_ATTR",$name);

*Note* : La méthode `set` permet d'insérer du 
XML dans l'ODT, comme dans l'exemple ci-dessous :

    [php]
    $society="Bob <text:line-break/> Compagnie" ;
    $this->lay->set("SOCIETY",$society) ; // insertion d'un retour à la ligne

*Note* : La valeur `null` est considérée comme non-instanciée, comme dans
l'exemple ci-dessous :

    [php]
    $this->lay->set("KEY",null) ; // [KEY] non remplacé
    $this->lay->set("KEY",'') ; // [KEY] effacé


## Affectation des répétables {#core-ref:9c954528-4c99-4423-ace2-0c445bc698ca}

Les templates ODT peuvent gérer les éléments multiples. Le chapitre [répétables
des vues ODT][odt_repeat] indique les différentes possibilités d'emplacement des
répétables.

L'affectation de valeurs multiples passe par deux méthodes :

* `OOoLayout::setColumn` : Cette méthode prend en entrée une clef et un array.
Si la clef est déclarée dans une liste ou un tableau alors une entrée est 
ajoutée par valeur dans le tableau,
* `OOoLayout::setRepeatable` : Cette méthode prend une matrice en entrée (array
de array). La méthode rend alors toutes les colonnes du tableau à la même 
taille que la plus grande des colonnes et ajoute les colonnes ainsi créées 
grâce à `OOoLayout::setColumn`.

    [php]
    /**
    * @templateController
    */
    public function viewtest() {
        $repeatable[] = array( 
            "V_X1"=>'A',
            "V_X2"=>'1',
            "V_X3"=>"La"
        );
        $repeatable[] = array( 
            "V_X1"=>'B',
            "V_X4"=>'2',
            "V_X3"=>"Si"
        );
        $repeatable[]=array(
            "V_X1"=>'C',
            "V_X2"=>'3',
            "V_X3"=>"Do"
        );
        $this->lay->setRepeatable($repeatable);
        
        $columnsValue = array('a','b','c','d');
        $this->lay->setColumn("V_XXX",$columnsValue);
    }

Le template :
![ template ](advanced/template/multiple_template.png)

donne le fichier :
![ résultat ](advanced/template/multiple_generated.png)


## Utilisation du type Htmltext {#core-ref:74d95597-be61-4ed5-b768-e4a78b6882a9}

Il n'y a pas de concordances entre les balises HTML et l'openText. Seul un sous
ensemble des balises HTML est supporté, ci-dessous est présentée la table 
d'équivalence présentant les balises supportées et leur équivalent ODT.

| balise HTML |     balise ODT     |                description                 |                                                                                                                                                    restriction                                                                                                                                                    |
| :---------- | :----------------- | :----------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `p`         | `text:p`           | Insère un paragraphe                       | La balise `<p>` doit être au premier niveau.                                                                                                                                                                                                                                                                      |
| `div`       | `text:p`           | Insère un paragraphe                       | Pour les `<div>` de premier niveau.                                                                                                                                                                                                                                                                               |
| `div > div` | `text:span`        | Insère un texte                            | Pour les `<div>` de niveau inférieur.                                                                                                                                                                                                                                                                             |
| `br`        | `text:line-break`  | Insère un retour à la ligne                | Doit être dans une balise `<p>`                                                                                                                                                                                                                                                                                   |
| `em/i`      | `text:span`        | Insèrent un texte avec le style "italique" | Doit être dans une balise `<p>`                                                                                                                                                                                                                                                                                   |
| `strong/b`  | `text:span`        | Insèrent un texte avec le style "gras"     | Doit être dans une balise `<p>`                                                                                                                                                                                                                                                                                   |
| `u`         | `text:span`        | Insère un texte avec le style "souligné"   | Doit être dans une balise `<p>`                                                                                                                                                                                                                                                                                   |
| `sup`       | `text:span`        | Insère un texte avec le style "exposant"   | Doit être dans une balise `<p>`                                                                                                                                                                                                                                                                                   |
| `sub`       | `text:span`        | Insère un texte avec le style "index"      | Doit être dans une balise `<p>`                                                                                                                                                                                                                                                                                   |
| `a`         | `text:a`           | Insère un hyperlien                        | Doit être dans une balise `<p>`                                                                                                                                                                                                                                                                                   |
| `h1`        | `text:h`           | Insère un titre de niveau 1                | Le style du texte sera celui de "Titre 1"                                                                                                                                                                                                                                                                         |
| `h2`        | `text:h`           | Insère un titre de niveau 2                | Le style du texte sera celui de "Titre 2"                                                                                                                                                                                                                                                                         |
| `h3`        | `text:h`           | Insère un titre de niveau 3                | Le style du texte sera celui de "Titre 3"                                                                                                                                                                                                                                                                         |
| `h4`        | `text:h`           | Insère un titre de niveau 4                | Le style du texte sera celui de "Titre 4"                                                                                                                                                                                                                                                                         |
| `li`        | `text:list-item`   | Insère un élément de liste                 | Doit être dans une balise `<ul>` ou `<ol>`                                                                                                                                                                                                                                                                        |
| `ul`        | `text:list`        | Insère une liste à puces non numérotée     | Doit être dans une balise `<p>`                                                                                                                                                                                                                                                                                   |
| `ol`        | `text:list`        | Insère une liste à puces numérotée         | Doit être dans une balise `<p>`                                                                                                                                                                                                                                                                                   |
| `table`     | `table:table`      | Insère un tableau                          |                                                                                                                                                                                                                                                                                                                   |
| `tr`        | `table:table-row`  | Insère une rangée tableau                  |                                                                                                                                                                                                                                                                                                                   |
| `th`        | `table:table-cell` | Insère une cellule entête de               |                                                                                                                                                                                                                                                                                                                   |
| `td`        | `table:table-cell` | Insère une cellule tableau                 |                                                                                                                                                                                                                                                                                                                   |
| `img`       | `draw:frame`       | Insère une image                           | L'url de cette image doit être absolue et accessible depuis l'éditeur de texte. Seules les images présente sur les paragraphes de premier niveau sont prises en compte. Pas d'image dans les cellule de tableau. La taille n'est pas configurable. C'est la taille d'origine de l'image qui sera prise en compte. |


### Exemple 1 - Paragraphe {#core-ref:cdcb4bad-9296-4f70-949f-e4ffdfef9fca}

Le code HTML

    [html]
    <p>Bonjour</p>

donne le code XML suivant :

    [xml]
    <text:section text:style-name="Secttxt_text" text:name="Sectiontxt_text" aid="txt_text">
      <text:p>Bonjour</text:p>
    </text:section>


### Exemple 2 - Liste à puces {#core-ref:ecf28987-1e1b-400c-8655-97dec99be44e}

Le code HTML

    [html]
    <ul>
      <li>élement 1</li>
      <li>élement 2</li>
    </ul>

donne le code XML suivant :

    [xml]
    <text:section text:style-name="Secttxt_text" text:name="Sectiontxt_text" aid="txt_text">
      <text:list text:style-name="LU">
        <text:list-item>
          <text:p text:style-name="L1">élement 1</text:p>
        </text:list-item>
        <text:list-item>
          <text:p text:style-name="L1">élement 2</text:p>
        </text:list-item>
      </text:list>
    </text:section>


### Exemple 3 - Division {#core-ref:f2c3eb19-197b-4b94-bf99-657b8ca3e593}

Le code HTML

    [html]
    <div>Bonjour</div>

donne le code XML suivant :

    [xml]
    <text:section text:style-name="Secttxt_text" text:name="Sectiontxt_text" aid="txt_text">
      <text:p>Bonjour</text:p>
    </text:section>


### Exemple 4 - Gras et italique {#core-ref:e1f4bd8f-739e-41d6-9977-7a64dac2c4cb}

Le code HTML

    [html]
    <div>
      <strong>Bonjour</strong>
      <br/>
      <em>le monde</em>
    </div>

donne le code XML suivant :

    [xml]
    <text:section text:style-name="Secttxt_text" text:name="Sectiontxt_text" aid="txt_text">
      <text:p>
        <text:span text:style-name="Tbold">Bonjour</text:span>
        <text:line-break/>
        <text:span text:style-name="Titalics">le monde</text:span>
      </text:p>
    </text:section>


### Exemple 5 - Titre de niveau {#core-ref:a0396f8f-8bef-4d50-b9aa-4621e400d289}

Le code HTML

    [html]
    <h1>Titre 1</h1>
    <h2>Titre 2</h2>
    <p>Mon paragraphe</p>

donne le code XML suivant :

    [xml]
    <text:section text:style-name="Secttxt_text" text:name="Sectiontxt_text" aid="txt_text">
      <text:h text:outline-level="1">Titre 1</text:h>
      <text:h text:outline-level="2">Titre 2</text:h>
      <text:p>Mon paragraphe</text:p>
    </text:section>

### Exemple 6 {#core-ref:2f4767c9-0358-40a6-8a7a-1de9fcd090b1}

Le code HTML

    [html]
    <table>
      <tr>
        <td>Cellule un</td>
        <td>Cellule deux</td>
      </tr>
    </table>

donne le code XML suivant :

    [xml]
    <text:section text:style-name="Secttxt_text" text:name="Sectiontxt_text" aid="txt_text">
      <table:table table:name="Table1" table:style-name="Table1">
        <table:table-row>
          <table:table-cell table:style-name="Table1.A1">
            <text:p>Cellule un</text:p>
          </table:table-cell>
          <table:table-cell table:style-name="Table1.A1">
            <text:p>Cellule deux</text:p>
          </table:table-cell>
        </table:table-row>
      </table:table>
    </text:section>

### Exemple 7 - Cas d'erreur {#core-ref:5cc6bc2a-73b7-4923-87ee-5046384c97d9}

Le code HTML

    [html]
    <div>
      <div class="my-class">Bonjour</div>
      <div>le monde</div>
    </div>

donne le code XML suivant :

    [xml]
    <text:section text:style-name="Secttxt_text" text:name="Sectiontxt_text" aid="txt_text">
      <text:p>
        <text:p>Bonjour</text:p>
        <text:p>le monde</text:p>
      </text:p>
    </text:section>

Ce code XML n'est pas conforme au schéma d'un document openDocument Text.
Un paragraphe ne peut pas contenir d'autres paragraphes.





<!-- link -->
[odt_limitation]:       #core-ref:b2f63c3f-9f26-47f6-8172-00c23b6a9948
[odt_repeat]:           #core-ref:9287cbe8-a6ca-41f9-9547-b7a970ae6584
[classlayout]:          #core-ref:9f9edc1b-17a5-4f54-86ee-69e33016fe18