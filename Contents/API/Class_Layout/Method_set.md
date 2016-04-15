# Layout::set() {#core-ref:812c30ed-11cb-4b59-84d2-ba10e4ab7e88}

<div markdown="1" class="short-description">

La méthode `set()` permet de spécifier les valeurs des balises (e.g. `[TAG]`)
du template.

</div>

## Description {#core-ref:bae968e1-6cc1-4a63-b29a-e6a12e88a6af}

    [php]
    void set ( string $tag, string $val )

### Avertissements {#core-ref:d14189fa-d430-4772-b38d-f389737993f1}

Les valeurs ne sont pas encodées. Si vous avez un template HTML, il faut utiliser
la méthode [eSet][eSet] pour encoder les valeurs si ce n'est pas un fragment
HTML.

Si la valeur contient une référence à une [zone][zone], (exemple :  "`[ZONE
MY:MYZONE]`"), la zone sera interprétée au contrainte de la méthode
[`eSet`][eset].

## Liste des paramètres {#core-ref:760dba7d-0669-43e4-999d-cb829bfdb820}

(string) `tag`
:   Le nom de la balise à affecter. Le nom de la balise est spécifié sans les
    crochets ouvrant (`[`) et fermant (`]`).

(string) `val`
:    La valeur à affecter à la balise.

## Valeur de retour {#core-ref:98b115b7-4e53-42c8-a914-d2835d3ca452}

Aucune.

## Erreurs / Exceptions {#core-ref:16c4f1cd-450f-428b-aa18-9d4db9986dbc}

Aucune.

## Historique {#core-ref:1c0fcbca-2575-4f3d-b90c-03807cc46880}

Aucun.

## Exemples {#core-ref:0102bf3e-41f8-4219-b73c-bc0bb87692cd}

- Exemple #1

Fichier de template `FOO/Layout/bar.xml` :

    [xml]
    <div>
    <p>Bonjour [NOM]&nbsp;[PRENOM],</p>
    <p>La température est actuellement de [TEMPC]°C.</p>
    </div>

Contrôleur :

    [php]
    $nom    = "Doe";
    $prenom = "John";
    $tempC  = "20";
    
    $layout->set("NOM",    htmlspecialchars($nom));
    $layout->set("PRENOM", htmlspecialchars($prenom));
    $layout->set("TEMPC",  sprintf("%02d",$temp));
    
    print $layout->gen();

## Notes {#core-ref:ce7f6410-ff79-4169-b9a5-c09b77468d06}

Pour se prémunir de toute attaque du type [XSS][XSS] (Cross-site scripting) il
vous faudra potentiellement échapper les valeurs en fonction du contexte dans
lesquelles elles seront utilisés.

Par exemple, pour un fragment (ou un document) HTML il vous faudra utiliser la
la méthode [`eSet()`][eset] ou la fonction PHP
[`htmlspecialchars()`][htmlspecialchars] afin d'échapper les caractères qui
pourraient être utilisés pour injecter du code HTML (ou JavaScript) dans le
résultat du template.



## Voir aussi {#core-ref:a48c9b01-686c-44de-b28f-f81828d1553f}

- Méthode [`Layout::setBlockData()`][setBlockData]
- Méthode [`Layout::eSet()`][eSet]

<!-- links -->
[setBlockData]: #core-ref:fb8a6d38-0bc7-469b-97d3-7cb8d6d3ea4b
[htmlspecialchars]: http://docs.php.net/manual/fr/function.htmlspecialchars.php "Définition sur php.net"
[XSS]: http://fr.wikipedia.org/wiki/XSS "Définition sur wikipédia"
[eset]:     #core-ref:2696710a-f491-4887-b953-e08d918ef4fb
[zone]:     #core-ref:28f377e2-b9ee-4427-b2ef-7134eed2bf72