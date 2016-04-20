# Layout::eSet()  {#core-ref:2696710a-f491-4887-b953-e08d918ef4fb}

<div markdown="1" class="short-description">

La méthode `eSet()` permet de spécifier les valeurs des balises (e.g. `[TAG]`)
de template HTML ou XML.<span class="flag release from">3.2.12</span>

</div>

## Description  {#core-ref:2c99cfbe-e12c-44a7-a23a-3a6bd8e106d7}

    [php]
    void eSet ( string $tag, string $val )

La différence entre cette méthode et la méthode [`set()`][set] est qu'elle
encode les valeurs lors de l'insertion. L'encodage est réalisée par la 
fonction PHP [`htmlspecialchars()`][htmlspecialchars].

Liste des caractères encodés :

| Caractère |  Encodage  |
| --------- | ---------- |
| "         | &amp;quot; |
| '         | &amp;039;  |
| &gt;      | &amp;gt;   |
| &lt;      | &amp;lt;   |

### Avertissements  {#core-ref:95d2589c-7077-4b00-b999-43dafa85ce05}

Cette méthode doit être utilisée si le champ est entre `'` (quote) ou `"`
(double-quote).

Si la valeur contient une référence à une [zone][zone], (exemple :  "`[ZONE
MY:MYZONE]`"), la zone ne sera pas interprétée.

## Liste des paramètres  {#core-ref:df082e35-f98b-4e8b-b199-34889eb1a71e}

(string) `tag`
:   Le nom de la balise à affecter. Le nom de la balise est spécifié sans les
    crochets ouvrant (`[`) et fermant (`]`).

(string) `val`
:    La valeur à affecter à la balise.

## Valeur de retour  {#core-ref:4b609a3f-0b90-4136-adf1-9e904145a964}

Aucune.

## Erreurs / Exceptions  {#core-ref:8a2d23bf-def2-4b15-8719-5fe2e2d7c9f1}

Aucune.

## Historique  {#core-ref:37fdd4bc-2f52-4038-b012-76e5203107a6}

Aucun.

## Exemples  {#core-ref:506e6bca-228f-4ff8-9e25-40289776c9c9}

- Exemple #1

Fichier de template `FOO/Layout/bar.xml` :

    [html]
    <div>
        <p data-name="[NOM]" data-firstname="[PRENOM]">
          [Message]
        </p>
    </div>

Contrôleur :

    [php]
    $nom    = "Doe";
    $prenom = 'John dit "l\'inconnu"';
    $msg  = "Écart admissible : 20 < x < 30";
    
    $layout->eSet("NOM",    $nom);
    $layout->eSet("PRENOM", $prenom);
    $layout->eSet("MESSAGE",  $msg);
    
    print $layout->gen();

Résultat :

    [xml]
    <div>
        <p data-name="Doe" data-firstname="John dit &quot;l&#039;inconnu&quot;>
          Écart admissible : 20 &lt; x &lt; 30
        </p>
    </div>

## Notes  {#core-ref:7aa15b22-105e-4026-9991-1ca9171280b5}

Attention, dans les cas de parties d'url, il faut utiliser la fonction 
[`urlencode`][urlencode] au lieu de [`htmlspecialchars`][htmlspecialchars]
afin de garantir l'intégrité de l'url. Dans ce cas, il faut utiliser la méthode
[`Layout::set()`][set] et appeler manuellement `urlencode`.

## Voir aussi  {#core-ref:a636b518-c3b2-4f6c-9dff-0e50c9337cd1}

- Méthode [`Layout::set()`][set]
- Chapitre [Encodage pour format HTML][encode]

<!-- links -->
[setBlockData]: #core-ref:fb8a6d38-0bc7-469b-97d3-7cb8d6d3ea4b
[htmlspecialchars]: http://docs.php.net/manual/fr/function.htmlspecialchars.php "Définition sur php.net"
[urlencode]: http://docs.php.net/manual/fr/function.urlencode.php "Définition sur php.net"
[XSS]: http://fr.wikipedia.org/wiki/XSS "Définition sur wikipédia"
[set]:     #core-ref:812c30ed-11cb-4b59-84d2-ba10e4ab7e88
[encode]:   #core-ref:751e00d9-eec7-41b7-835f-14df8c0b7821