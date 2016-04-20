# Layout::gen() {#core-ref:d5100f38-c9fb-4832-ac2b-ed7d357b6c67}

<div markdown="1" class="short-description">

La méthode `gen()` permet de lancer la génération du template après avoir
positionné les valeurs des balises à l'aide des méthodes [`set()`][set] et
[`setBlockData()`][setBlockData].

</div>

## Description {#core-ref:6516236c-cb73-4c52-a278-de511956b8bb}

    [php]
    string gen ( void )

Le résultat est retourné sous la forme d'une chaîne de caractères.

### Avertissements {#core-ref:c68f4545-995e-41f9-8194-f10fd5ee0cc8}

Aucun.

## Liste des paramètres {#core-ref:1cd16c2a-5bca-44a2-8e73-0dbaf61392dd}

Aucun.

## Valeur de retour {#core-ref:849401e1-6476-4a4e-9eee-5bc3ae97b8cd}

La méthode retourne une chaîne de caractère contenant le résultat de
l'évaluation du template.

## Erreurs / Exceptions {#core-ref:cbd7e6db-8ce2-488a-81be-9cdf1651bbb4}

Aucune.

## Historique {#core-ref:5cd6980d-1ab2-40e2-94b2-9b8e281db3e8}

Aucun.

## Exemples {#core-ref:43f2b0ce-8117-4f19-b248-2f6ee8c7fb14}

- Exemple #1

Dans cet exemple, on va utiliser le mécanisme de template pour générer
dynamiquement des balises de blocs `[BLOCK XXX]...[ENDBLOCK XXX]`.

Fichier de template `MOVIES/Layout/movies.xml` :

    [xml]
    <ul>
    [BLOCK MOVIES]
    <li>
            <p><span class="movie_title">[MOVIE_TITLE]</span> (<span class="movie_date">[MOVIE_DATE]</span>)</p>
            <p>Characters:
            <ul>[BLOCK [CHARACTERS]]
                    <li>[CHARACTER]</li>[ENDBLOCK [CHARACTERS]]
            </ul>
            </p>
    </li>
    [ENDBLOCK MOVIES]
    </ul>
    EOF;

Contrôleur :

    [php]
    $layout = new Layout("MOVIES/Layout/movies.xml");
    
    $layout->setBlockData(
        'MOVIES',
        array(
            array(
                'MOVIE_TITLE' => htmlspecialchars('Star Wars: A New Hope'),
                'MOVIE_DATE'  => '1977',
                'CHARACTERS' => 'CHARACTERS_SW_A_NEW_HOPE'
            ),
            array(
                'MOVIE_TITLE' => htmlspecialchars('Star Wars: The Empire Strikes Back'),
                'MOVIE_DATE'  => '1980',
                'CHARACTERS' => 'CHARACTERS_SW_THE_EMPIRE_STRIKES_BACK'
            ),
            array(
                'MOVIE_TITLE' => htmlspecialchars('Star Wars: Return of the Jedi'),
                'MOVIE_DATE'  => '1983',
                'CHARACTERS' => 'CHARACTERS_SW_RETURN_OF_THE_JEDI'
            )
        )
    );
    
    $layout->setBlockData(
        'CHARACTERS_SW_A_NEW_HOPE',
        array(
            array(
                'CHARACTER' => 'Chewbacca'
            ),
            array(
                'CHARACTER' => 'Luke Skywalker'
            ),
            array(
                'CHARACTER' => 'Han Solo'
            )
        )
    );
    
    $layout->setBlockData(
        'CHARACTERS_SW_THE_EMPIRE_STRIKES_BACK',
        array(
            array(
                'CHARACTER' => 'Yoda'
            ),
            array(
                'CHARACTER' => 'Boba Fett'
            )
        )
    );
    $layout->setBlockData(
        'CHARACTERS_SW_RETURN_OF_THE_JEDI',
        array(
            array(
                'CHARACTER' => 'Emperor Palpatine'
            ),
            array(
                'CHARACTER' => 'Wicket'
            )
        )
    );
    
    print $layout->gen();

Le premier niveau de blocs `[BLOCK MOVIES]` va dupliquer les chaînes
`[BLOCK [CHARACTERS]]` pour donner de nouveau blocs

-   `[BLOCK CHARACTERS_SW_A_NEW_HOPE]`,
-   `[BLOCK CHARACTERS_SW_THE_EMPIRE_STRIKES_BACK]` et
-   `[BLOCK CHARACTERS_SW_RETURN_OF_THE_JEDI`.

Ensuite, ces blocs `[BLOCK CHARACTERS_SW_xxx]` sont évalués avec le contenu de
leur `setBlockData('CHARACTERS_SW_xxx')` respectif.

Résultat :

    [html]
    <ul>
    
    <li>
        <p><span class="movie_title">Star Wars: A New Hope</span> (<span class="movie_date">1977</span>)</p>
        <p>Characters:
        <ul>
                <li>Chewbacca</li>
                <li>Luke Skywalker</li>
                <li>Han Solo</li>
        </ul>
        </p>
    </li>
    
    <li>
        <p><span class="movie_title">Star Wars: The Empire Strikes Back</span> (<span class="movie_date">1980</span>)</p>
        <p>Characters:
        <ul>
                <li>Yoda</li>
                <li>Boba Fett</li>
        </ul>
        </p>
    </li>
    
    <li>
        <p><span class="movie_title">Star Wars: Return of the Jedi</span> (<span class="movie_date">1983</span>)</p>
        <p>Characters:
        <ul>
                <li>Emperor Palpatine</li>
                <li>Wicket</li>
        </ul>
        </p>
    </li>
    
    </ul>

## Notes {#core-ref:0c1956da-0df5-448e-84aa-074338edafb2}

L'expansion des balises est faite dans l'ordre suivant :

1. balise de bloc (e.g. `[BLOCK xxx]...[ENDBLOCK xxx]`) ;
2. balise conditionnelle (e.g. `[IF xxx]...[ENDIF xxx]`, `[IFNOT xxx]...[ENDIF xxx]`) ;
3. balise d'internationalisation de texte (e.g. `[TEXT:xxx]`) ;
4. balise de clef atomique (e.g. `[xxx]`) définie via les méthodes [`eSet()`][eset] ou [`set()`][set] ;
5. balise de paramètre applicatif (si $action présent dans le [constructeur][layoutconstruct]).
6. balise de référence d'image (e.g. `[IMG:xxx]`) ;
7. balise de zone (e.g. `[ZONE xxx]`) ;
8. balise de référence ou de code JavaScript (e.g. `[JS:REF]`, `[JS:CODE]`) ;
9. balise de référence ou de code CSS (e.g `[CSS:REF]`, `[CSS:CODE]`).

## Voir aussi {#core-ref:41d41bf4-a619-4db7-9d33-dd630d866244}

- Méthode [`Layout::set()`][set]
- Méthode [`Layout::setBlockData()`][setBlockData]
- [Liste des balises utilisables en texte][tag_list_texte]

<!-- links -->
[set]:              #core-ref:812c30ed-11cb-4b59-84d2-ba10e4ab7e88
[setBlockData]:     #core-ref:fb8a6d38-0bc7-469b-97d3-7cb8d6d3ea4b
[tag_list_texte]:   #core-ref:32dea245-37e6-4a4c-a65e-06c577c0effa
[eset]:             #core-ref:2696710a-f491-4887-b953-e08d918ef4fb
[layoutconstruct]:  #core-ref:d0038e8d-b915-4d84-ac28-4f6c0aeb20a0