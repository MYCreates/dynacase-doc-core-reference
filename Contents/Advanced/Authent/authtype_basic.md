# Authentification HTTP Basic {#core-ref:d53b0cf8-8a73-4a72-a8cc-dd60636363f6}

L'Authenticator `basic` ne présente aucune interface pour la saisie du login et
du mot de passe, mais se base sur le mécanisme d'authentification [HTTP
Basic][http_basic].

La sélection du mode d'authentification `basic` s'effectue avec la variable GET
`authtype=basic`, qui permet alors d'indiquer de ne pas utiliser l'Authenticator par défaut (`html` par exemple).

Exemple d'URL avec Authenticator `basic` :

    /index.php?authtype=basic&app=FOO&action=BAR

## Accès avec mot de passe  {#core-ref:4b928227-d235-46ea-822e-7726f04a3564}

<span class="flag from release inline">3.2.23</span> 
L'accès peut être effectué en indiquant le login et le mot de passe dans le
header HTTP.

    GET ?app=CORE&action=BLANK
    Authorization: Basic am9obi5kb2U6c2VjcmV0

Dans ce cas, le header doit contenir la méthode utilisée (Basic) suivi de la
représentation en Base64 du nom de l'utilisateur et du mot de passe séparés par
le caractère « : » (deux-points).

En console, l'accès peut être fait avec le programme `curl` :

    curl   --user  "john.doe:secret" "http://www.example.net/?app=CORE&action=BLANK"



<!-- links -->
[http_basic]: http://en.wikipedia.org/wiki/Basic_access_authentication