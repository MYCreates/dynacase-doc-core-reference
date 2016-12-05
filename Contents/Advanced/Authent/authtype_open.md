# Authentification par jetons {#core-ref:9edc8f2e-6929-11e2-8610-0021e9fffec1}

L'authentification en mode `open` permet d'effectuer une requête authentifiée
sans fournir  l'identifiant ni le mot de passe mais en fournissant un jeton
d'authentification.

Cette clef d'authentification est associée à un utilisateur défini. Elle permet
d'exécuter les [actions][actiondesc] qui sont déclarées "openaccess" (valeur
"Y").

    [php]
    // Fichier MYAPP.app
    $action_desc = array(
        array(
            "name" => "MY_OPENACTION",
            "acl" => "MY_ACL",
            "openaccess" => "Y",
            "short_name" => "Open access test"
        )
    );


## Obtenir un jeton d'authentification {#core-ref:d6e188ac-814e-4566-9155-2591d2ee5e9c}

La méthode `Account::getUserToken` permet de générer un jeton pour un
utilisateur donné.

    [php]
    string Account::getUserToken(int   $expireDelay = -1, 
                                 bool  $oneshot     = false, 
                                 array $context     = array(),
                                string $description = "",
                                  bool $forceCreate=false)

Cette méthode retourne un jeton sous forme de chaîne de caractères hexadécimaux.

$expireDelay (int): Délai de validité du jeton en secondes.
:   Mettre "-1" (ou `false`) pour indiquer un délai infini.  
    <span class="flag from release inline">3.2.23</span> -1 est supporté depuis cette version. 
    Dans les versions antérieures, il fallait indiquer "`false`", pour l'infini.

$oneshot (bool): Mettre "true" pour indiquer que le jeton sera valide une seule fois
:   Le jeton est supprimé une fois consommée.

$context (array): Tableau indexé (clef/valeur)
:   Indique que les paramètres qui sont obligatoires dans l'url

$description (string): Texte informatif sur le jeton
:   Visible lors de l'utilisation de la gestion des jetons dans le centre d'administration

$forceCreate (bool): <span class="flag from release inline">3.2.23</span> Pour forcer la création d'un nouveau jeton
:   Si cette méthode est appelée avec les
mêmes arguments plusieurs fois et avec l'option `oneshot` à `false` et 
`forceCreate` à `false`, le jeton
n'est pas créé si un jeton avec les mêmes caractéristiques existe déjà. Dans ce
cas, c'est le même jeton qui est fourni.

Exemple : Construction d'un jeton utilisable à vie pour l'utilisateur "john.doe"

    [php]
    $a=new account();
    $a->setLoginName("john.doe");
    
    $t=$a->getUserToken();
    print $t; // > cad68f8e1848f12df16e44857c595f9a72124f41

Ce jeton permet d'exécuter toutes les actions déclarées en "openaccess".


Exemple : Construction d'un jeton utilisable à vie pour l'utilisateur "john.doe"
restreinte à l'action "MY_OPENACTION" de l'application "MYAPP"

    [php]
    $a=new account();
    $a->setLoginName("john.doe");
    
    $t=$a->getUserToken(false, false, 
        ["app"=>"MYAPP", "action"=>"MY_OPENACTION"]);



Exemple : Construction d'un jeton utilisable pour une heure pour l'utilisateur "john.doe"
restreinte à l'action "MY_OPENACTION" de l'application "MYAPP"

    [php]
    $a=new account();
    $a->setLoginName("john.doe");
    
    $t=$a->getUserToken(3600, false, 
        ["app"=>"MYAPP", "action"=>"MY_OPENACTION"]);



## Requête pour exécuter une action avec l'authentification "open" {#core-ref:3e1cd72d-1440-4279-90a8-a11dd8e9cadb}

Le jeton doit être indiqué dans l'url avec la variable `dcpopen-authorization`. Elle peut
aussi être indiquée par une variable dans un formulaire (méthode HTTP POST).


L'url doit contenir la variable "dcpopen-authorization".


Exemple d'URL :

    ?dcpopen-authorization=e2bb65612c70e7ac78d5ccbfe12aa234&app=FOO&action=BAR


<span class="flag from release inline">3.2.23</span>La variable `privateid` utilisée dans les
versions précédentes est dépréciée au profit de la variable `dcpopen-authorization`.
Si la variable `privateid` est utilisée alors il faut aussi indiquer explicitement la variable "authtype`.

    (déprécié) ?authtype=open&privateid=e2bb65612c70e7ac78d5ccbfe12aa234&app=FOO&action=BAR


Dans ce cas, le cookie de session, s'il est émis, est ignoré.

Le paramètre système de "CORE_OPENURL" est utilisé pour construire les urls.  Il
doit contenir l'url d'accès à l'application. S'il n'est pas valué (par défaut), c'est
la valeur du paramètre "CORE_URLINDEX" qui est utilisé.

Le jeton peut aussi être indiqué dans le header HTTP :

    GET ?app=FOO&action=BAR
    Authorization: DcpOpen e2bb65612c70e7ac78d5ccbfe12aa234

Dans ce cas, le paramètre `authtype` est optionnel. 

Par contre, si le jeton est indiqué et qu'un cookie de session est émis, le
cookie de session est ignoré. Le terme "DcpOpen" est le "HTTP Authentication
Scheme" pour identifier le mode d'autorisation.

<span class="flag from release inline">3.2.18</span> L'authentification par
jeton ne donne plus lieu à l'émission d'un cookie de session
(l'authentification étant portée par le jeton et non par une session HTTP). Par
le passé, l'envoi d'une requête avec authentification par jeton déclenchait
systématiquement l'émission d'un nouveau cookie de session bien que ce dernier
ne soit pas utilisé.

Pour plus de détails sur la création des jetons : voir la documentation du
fichier de la classe `Class.UserToken.php`.

En cas de jeton invalide, le statut HTTP retourné est 403.

[actiondesc]:   #core-ref:e67d8aeb-939c-46e3-9be8-6fc3ba75ebc2