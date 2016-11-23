# Dynacase en ligne de commande {#core-ref:1566c46d-a53d-44cf-8c3f-0d0e21c0b117}

L'exécution de programmes permettant de gérer les documents et le paramétrage en
général peut être fait en ligne de commande. Le point d'entrée de la ligne de
commande dans Dynacase est le script `wsh.php`.

## `wsh.php` {#core-ref:bab8c1c9-fe71-4629-9773-5cd67a8693bf}

Ce script permet de :

*   lancer des [scripts][wsh_script],
*   exécuter des [actions][wsh_action].

Son aide est disponible au moyen de l'option `--help` :

    ./wsh.php --help
    usage   wsh.php --app=APPLICATION --action=ACTION [--ARG=VAL] ...:  execute an action
            wsh.php --api=API [--ARG=VAL] ....   :  execute an api function
            wsh.php --listapi                     : view api list

Note :

* Le script `wsh.php` doit être exécuté, en ligne de commande, sous le même
  compte utilisateur que celui du serveur Apache (afin que les fichiers
  produits soient accessibles par Dynacase en mode Web) et ne doit en aucun cas
  être lancé sous le compte utilisateur`root`.

### Utilisateur effectuant l'opération {#core-ref:f266fa37-8d4c-45f9-806f-154861bfc547}

Par défaut, les opérations sont lancées sous l'identité Dynacase de
l'utilisateur *Master Default* (administrateur principal - compte `admin`). Il
est possible de changer cette identité au moyen de l'option `--userid`, qui
prend comme paramètre le *login* d'un utilisateur Dynacase, ou son *identifiant
système* Dynacase.

### Passage d'arguments {#core-ref:fa9210eb-5bad-4867-a287-e732afc02251}

#### Arguments simples {#core-ref:4b368074-f30c-49c3-8c34-6d2d9a9b2ddf}

Il est possible de passer des arguments nommés en rajoutant des options avec la
notation `--argumentName=argumentValue`. Le nom de l'argument correspond au nom
de l'option. Ainsi, `--foo=bar` assignera la valeur `"bar"` à l'argument `foo`.

**Attention** : les arguments ne sont pas typés : ils sont systématiquement
envoyés au format texte. Aussi, lors de l'utilisation du paramètre `--foo=true`,
le script appelé recevra l'argument `"true"` (string) et non pas le booléen
`true`.

#### Arguments multivalués {#core-ref:6cad24b2-bce8-43b6-97bb-a34f4f27328d}

Si la valeur de l'argument comporte des espaces les double-quotes `"`ou les
quotes `'` peuvent être utilisées.

    ./wsh.php --api=my_test --my_number=3 --my_text="l'argument texte"

Le passage d'arguments multivalués se fait au moyen de la notation
`--argumentName=argumentValue` répétée autant de fois que nécessaire. Ainsi,
`--my_numbers[]=2 --my_numbers[]=4` assignera la valeur `array("2","4")` à
l'argument `my_numbers`.

Pour la récupération des arguments, se référer à la documentation de la
[classe `ApiUsage`][ApiUsage].

#### Arguments booléens {#core-ref:318ddb5d-f059-439f-a1f8-277fc9949fc9}

Enfin, il est possible de passer des arguments booléens avec la notation
`--argumentName` (sans valaur). Ainsi, `--foo` assignera la valeur `true` à
l'argument `foo`.

## Gestion des erreurs `wsh` {#core-ref:5ada82f5-0f47-4345-b99b-e33901d9bc3a}

Lorsqu'un shell non interactif[^core-ref:shell non interactif] sort en erreur,
un mail contenant le détail de l'erreur peut être envoyé.

Ce mail est envoyé uniquement si le paramètre [`CORE_WSH_MAILTO`][core-ref:CORE_WSH_MAILTO] est non vide.

## Exécuter des scripts avec _wsh_ {#core-ref:c47cdda0-0221-4dfc-ba14-56376e570372}

Exemple d'appel :

    ./wsh.php --api=cleanContext

Cela appellera le script `/API/cleanContext.php`.

La liste des scripts disponibles est obtenu au moyen de la commande *listapi* :

    ./wsh.php --listapi
    application list :
        - AppSwitcherSetCoreStartApp
        - DocRelInit
        - […]
        - vault_init
        - wdoc_graphviz

Afin de connaître l'usage d'un script, il est possible d'utiliser l'option
`--help` :

    ./wsh.php --api=cleanContext --help
    Clean base
    Usage :
        Options:
            --userid=<user system id or login name to execute function - default is (admin)>, default is '1'
            --real=<real (yes or no)>
            --help (Show usage)

## Exécuter des actions avec wsh {#core-ref:63832d9f-61a8-4846-a9d5-c34ee58de4a6}

Il est possible au moyen de *wsh* de lancer n'importe quelle action de n'importe
quelle application.

Exemple d'appel :

    ./wsh.php --app=MY_APP --action=MY_ACTION

Les arguments spécifiques de l'action doivent être indiqués au moyen de la
[syntaxe idoine][args]

Le résultat de l'action est retournée sur la sortie standard.

Exemple d'exécution de l'action `FDL_FAMILYSCHEMA` de l'application `FDL` :

    ./wsh.php --app=FDL --action=FDL_FAMILYSCHEMA --id=DIR > dir.xsd

Toute action ou tout script exécuté avec wsh n'utilise pas les sessions. Les
sessions, étant utilisées pour une connexion avec le client web, n'ont pas lieu
d'être utilisées.

## Retour d'erreur {#core-ref:982b9e0c-56ef-40c4-a8f8-0ae0826f07a2}

Le script retourne un *exit status* indiquant si une erreur s'est produite.
Lorsqu'une erreur se produit, son message est retourné sur la sortie standard
(*stdout*), et non sur la sortie d'erreur (*stderr*).

Codes retournés :

*   `0` : Pas d'erreur
*   `1` : Déclenchement d'une exception, appel à `Action::exitError()`, ou 
    action inconnue
*   `2` : Utilisateur inconnu ou non actif (option `--userid`)
*   `4` : Le script indiqué par `--api` est introuvable.

Exemple :

    $ ./wsh.php --api=pastrouve
    fichier API API/pastrouve.php non trouvé
    $ echo $?
    4

<span class="flag inline from release">3.2.17</span> Si le code retourné est `1`, le
message d'erreur est affiché avec la fonction [`error_log()`][error_log]. Par
défaut, l'affichage est effectué sur la sortie `stderr`. Ce message est
accompagné de la *callstack* qui est à l'origine de l'erreur.

Exemple :

    $ ./wsh.php --api=refreshDocuments --famid=389
        17208> Dynacase got an uncaught exception 'Dcp\Core\Exception' with message '{CORE0001} family 389 not exists' in file /var/www/tmp32/WHAT/Class.Action.php at line 651:
        17208> #0 /var/www/tmp32/API/refreshDocuments.php(74): Action->exitError('family 389 not ...')
        17208> #1 /var/www/tmp32/wsh.php(133): include('/var/www/tmp32/...')
        17208> #2 {main}
        17208> End Of Exception.
    $ echo $?
    1


Si le code est `2` ou `4`, le message d'erreur est affiché sur la sortie
standard (`stdout`).

## Écrire un script CLI {#core-ref:4df1314f-9fdd-4a7f-af37-a18cc39f3505}

Les scripts exécutables avec wsh doivent être des fichiers php présents dans le
sous-répertoire `./API` du contexte d'installation. Ces fichiers doivent porter
l'extension `php` (le *basename* du fichier détermine le *nom de l'api*).

Lors du lancement d'un script, la variable `$action` est initialisée avec un
objet de la [classe `Action`][classe_action]. L'autoloader est initialisé, et
l'intégralité des fonctions et méthodes de Dynacase peuvent être utilisées sans
contrainte spécifique.

<!-- links -->
[^core-ref:shell non interactif]: Il s'agit d'un script lancé via le `CLI PHP SAPI`, qui n'a ni `TTY`, ni `STDIN`, ni `STDOUT`, ni `STDERR`.

[ApiUsage]: #core-ref:dac6d107-3e77-48ba-8912-ffccd0061cbf
[args]: #core-ref:fa9210eb-5bad-4867-a287-e732afc02251
[classe_action]: #core-ref:29553eba-bcea-4baf-bef8-103c3a3510fa
[core-ref:CORE_WSH_MAILTO]: #core-ref:6457a9c1-8b3e-4fd1-913a-8f9e133fc7a4
[error_log]: http://php.net/manual/fr/errorfunc.configuration.php#ini.error-log "Configuration error_log"
[wsh_action]: #core-ref:63832d9f-61a8-4846-a9d5-c34ee58de4a6
[wsh_api]: #core-ref:c47cdda0-0221-4dfc-ba14-56376e570372
[wsh_script]: #core-ref:c47cdda0-0221-4dfc-ba14-56376e570372
