# Vue de rangée de tableau {#core-ref:9e76ac49-3b17-435b-ba25-a7122369be85}

Les vues de rangée de tableau permettent de mettre en forme un tableau. Cela est
particulièrement utile pour des tableaux contenant beaucoup d'attributs, ou dont
la structure est complexe. Il est ainsi possible de changer la disposition des
éléments pour, par exemple, regrouper plusieurs attributs dans la même colonne.

Contrairement aux vues de document, ou aux vues d'attribut, les vues de rangée
de tableau ne contiennent pas de code. Elles sont donc uniquement composées de :

*   un fichier de template, définissant la structure.

Par convention :

*   le fichier de template porte l'extension `.xml`,
*   son nom détermine le nom de la vue.

## Syntaxe du template de rangée de tableau {#core-ref:d9a89b1e-9792-49c3-b390-d358c2b78999}

Un template de vue de rangés de tableau est un fichier XML **valide**.
Il doit respecter la `DTD` suivante :

    [dtd]
    <!ELEMENT table (table-head*,table-body*)>
    
    <!ELEMENT table-head (cell)*>
    <!ELEMENT table-body (cell)*>
    
    <!ELEMENT cell ANY>
    <!ATTLIST cell
            class CDATA #IMPLIED
            style CDATA #IMPLIED>
    ]>

Voici un exemple de fichier valide (ici, la DTD est déclarée inline, mais elle
n'est pas obligatoire) :

    [xml]
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE table [
        <!ELEMENT table (table-head*,table-body*)>
        
        <!ELEMENT table-head (cell)*>
        <!ELEMENT table-body (cell)*>
        
        <!ELEMENT cell ANY>
        <!ATTLIST cell
                class CDATA #IMPLIED
                style CDATA #IMPLIED>
        ]>
    <table>
        <table-head>
            <cell>…</cell>
        </table-head>
        <table-body>
            <cell>…</cell>
        </table-body>
    </table>

*   Le bloc `table-head` définit le `thead` du tableau généré,
*   le bloc `table-body` définit une rangée `tr` du `tbody` du tableau généré.
*   Chaque balise `cell` définit une cellule et est donc retranscrite sous la
    forme d'une balise `td`.
    *   les attributs `style` et `class` de la balise `cell` sont transmis
        tels-quels à la balise `td` résultante,
    *   les autres attributs sont ignorés,
    *   le contenu de la balise `td` résultante est exactement le contenu de la
        balise `cell`. ce dernier doit donc être du xhtml valide.  
        **Attention** : puisque le document est interprété en *XML pur*, pour
        utiliser des entités HTML, il faut les déclarer dans la DTD (par
        exemple: `<!ENTITY nbsp "&#160;">`).

## Vue de consultation de rangée de tableau {#core-ref:d237e0d4-ca4e-4b9e-8249-45aef7d2ef0f}

La représentation spécifique de tableaux est insérée en lieu et place de la
représentation standard de tableau.

La représentation standard d'un tableau en consultation est :

    [html]
    <fieldset ><legend>Nom du tableau</legend>
        <table>
            <thead>
                <tr><th>Label 1</th><th>Label 2</th></tr>
            </thead>
            <tbody>
                <tr><td>Valeur 1.1</td><td>Valeur 2.1</td></tr>
                <tr><td>Valeur 1.2</td><td>Valeur 2.2</td></tr>
            </tbody>
        </table>
    </fieldset>

Afin de spécifier la vue d'attribut à utiliser en consultation, il faut utiliser
l'option `rowviewzone`.

### Utilisation des valeurs du document {#core-ref:553ce7f2-b0db-4986-b72b-09ccad1831b2}

Lors de l'utilisation des vues de rangés de tableau, les clés suivantes sont
instanciées :

*   `L_ATTRID` pour chaque attribut : le libellé (traduit) de l'attribut,
*   `V_ATTRID` pour chaque attribut : la valeur (au format HTML) de l'attribut.

**Attention** : les clés `V_ATTRID` respectent les visibilités : si la
[visibilité][visibility] est `H` (caché) ou `I` (invisible) ou 
`O` (écriture seule), les clés  `V_ATTRID` sont des chaînes vides. Les clés
`L_ATTRID` ne sont pas affectées par la visibilité.

De plus, il est possible, comme dans tous les templates, d'utiliser :

*   les paramètres globaux,
*   les chaînes traduisibles (syntaxe `[TEXT:…]`).

Exemple :

Le template suivant :

    [xml]
    <?xml version="1.0"?>
    <table>
      <table-head>
        <cell class="special">[TEXT:Identification]</cell>
        <cell style="background-color:[COLOR_C3]">[L_MY_NUMBERS]</cell>
      </table-head>
      <table-body>
        <cell style="background-color:[COLOR_B6]">
          <b>[V_MY_NUMBERID]</b><br/>[V_MY_NUMBERLABEL]
        </cell>
        <cell style="background-color:[COLOR_C5];text-align:center">
          [V_MY_NUMBERS]
        </cell>
      </table-body >
    </table>

produit pour l'exemple le code HTML suivant :

    [html]
    <fieldset ><legend>Nom du tableau</legend>
        <table>
            <thead>
                <tr><th class="special">Identification</th>
                    <th style="background-color:#E5DD57">Mes nombres</th></tr>
            </thead>
            <tbody>
                <tr><td style="background-color:#8EC5F4"><b>1.1</b><br/>numéro 1</td>
                    <td style="background-color:#EDE787;text-align:center">001</td></tr>
                <tr><td style="background-color:#8EC5F4"><b>1.2</b><br/>numéro 2</td>
                    <td style="background-color:#EDE787;text-align:center">002</td></tr>
            </tbody>
        <table>
    </fieldset>

Ce qui représente le tableau suivant :
<table>
    <thead>
        <tr><th class="special">Identification</th>
            <th  style="background-color:#E5DD57">Mes nombres</th></tr>
    </thead>
    <tbody>
        <tr><td style="background-color:#8EC5F4"><b>1.1</b><br/>numéro 1</td>
            <td style="background-color:#EDE787;text-align:center">001</td></tr>
        <tr><td style="background-color:#8EC5F4"><b>1.2</b><br/>numéro 2</td>
            <td style="background-color:#EDE787;text-align:center">002</td></tr>
    </tbody>
</table>

## Vue de modification de rangée de tableau {#core-ref:c0ed29f6-02e5-4ff2-8943-511a7d00c106}

Afin de spécifier la vue d'attribut à utiliser en modification, il faut utiliser
l'option `roweditzone`.

La représentation standard d'un tableau en modification est :

    [html]
    <fieldset ><legend>Nom du tableau</legend>
        <table>
            <thead>
                <tr><th class="tools"/>
                    <th colspan="2">Label 1</th>
                    <th colspan="2">Label 2</th></tr>
            </thead>
            <tbody>
                <tr><td>tools inputs</td>
                    <td>Champ n°1.1</td><td>Boutons Champ n°1.1</td>
                    <td>Champ n°1.2</td><td>Boutons Champ n°1.2</td>
                <tr><td>tools inputs</td>
                    <td>Champ n°2.1</td><td>Boutons Champ n°2.1</td>
                    <td>Champ n°2.2</td><td>Boutons Champ n°2.2</td></tr>
            </tbody>
            <tfoot>
                … autres champs cachés …
            </tfoot>
        </table>
    </fieldset>
    
### Utilisation des champs de saisie du document {#core-ref:bd24ca4f-181a-4073-9144-fc0b8cb10727}

Lors de l'utilisation des vues de rangés de tableau, les clés suivantes sont
instanciées :

*   `L_ATTRID` pour chaque attribut : le libellé (traduit) de l'attribut,
*   `V_ATTRID` pour chaque attribut : le champs de saisie correspondant à l'attribut,

**Attention** : les clés `V_ATTRID` respectent les visibilités : si la
[visibilité][visibility] est `H` (caché) ou `R` (lecture seule) les clefs sont
des champs cachés `<input type="hidden" />`.
Si la visibilité est `I` (invisible), les clés `V_ATTRID` sont des chaînes
vides. Les clés `L_ATTRID` ne sont pas affectées par la visibilité.

De plus, il est possible, comme dans tous les templates, d'utiliser :

*   les paramètres globaux,
*   les chaînes traduisibles (syntaxe `[TEXT:…]`).

Le template suivant :

    [xml]
    <?xml version="1.0"?>    
    <table>
        <table-head>
            <cell class="special">[TEXT:Identification]</cell>
            <cell style="background-color:[COLOR_C3]">[L_MY_NUMBERS]</cell>
        </table-head>
        <table-body>
            <cell style="background-color:[COLOR_B6]"> <b>[V_MY_ROMANNUMBER]</b>
                <br/>    
                [V_MY_NUMBERLABEL]
            </cell>
            <cell>
                <span>style="text-align:center">[V_MY_NUMBERS]</span>
            </cell>
        </table-body >
    </table>

produit pour l'exemple le code HTML suivant :

À la différence d'un tableau standard, les champs de saisie d'attribut ne sont
plus répartis sur deux cellules mais sur une seule cellule (plus de `colspan`)
dans l'entête de tableau.

    [html]
    <fieldset ><legend>Nom du tableau</legend>
        <table>
            <thead>
                <tr><th class="tools"/>
                    <th class="special">Identification</th>
                    <th style="background-color:#E5DD57">
                       Mes nombres</th></tr>
            </thead>
            <tbody>
                <tr><td>Tool inputs</td>
                    <td style="background-color:#8EC5F4">
                        <b><input name="_my_romannumber[]" value="1.1"/> </b><br/>
                        <input name="_my_numberlabel[]" value="numéro 1"/>
                        <input type="button" value="..." /><input type="button" value="x" />
                    </td>
                    <td><span style="text-align:center">
                      <input name="_my_numbers[]" value="1"/>
                      <input type="button" value="..." /><input type="button" value="x" />
                    </span></td></tr>
                <tr>...Deuxième rangée...</tr>
                
            </tbody>
            <tfoot>
                ... autres champs cachés ...
            </tfoot>
        </table>
    </fieldset>


Cet exemple produit la représentation suivante :

<fieldset style="border:solid 1px;padding:5px;"><legend style="margin-left:20px">Nom du tableau</legend>  
    <table>
        <thead>
            <tr><th class="tools"/>
                <th >Identification</th>
                <th  style="background-color:#E5DD57">
                   Mes nombres</th></tr>
        </thead>
        <tbody>
            <tr><td><input type="radio"/></td>
                <td style="background-color:#8EC5F4">
                    <b><input  value="MCMLXVIII"/> </b><br/>
                    <input value=" mille-neuf-cent-soixante-huit"/>
                    <input type="button" value="..." /><input type="button" value="x" />
                </td>
                <td><span style="text-align:center">
                  <input  value="1968"/>
                  <input type="button" value="..." /><input type="button" value="x" />
                </span></td></tr>
            <tr><td><input type="radio"/></td>
                <td style="background-color:#8EC5F4">
                    <b><input  value="MMXIII"/> </b><br/>
                    <input value="deux-mille-treize"/>
                    <input type="button" value="..." /><input type="button" value="x" />
                </td>
                <td><span style="text-align:center">
                  <input  value="2013"/>
                  <input type="button" value="..." /><input type="button" value="x" />
                </span></td></tr>
            
        </tbody>
    </table>
</fieldset>

<!-- links -->
[visibility]: #core-ref:3e67d45e-1fed-446d-82b5-ba941addc7e8