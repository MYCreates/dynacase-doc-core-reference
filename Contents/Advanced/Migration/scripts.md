# Scripts de pré-migration et de post-migration {#core-ref:fd259229-b279-469a-8e3c-d737fabcf9d5}

Ces scripts de migration ne peuvent être utilisés que dans le cadre de **mise à
jours d'application**.

Un module sans application ne peut pas utiliser cette mécanique de migration.

Deux types de scripts sont possibles :

* scripts de pré-migration (premigr) : les scripts de pré-migration (premigr) 
  sont prévus pour être exécutés avant que l'application et les familles en base
  de données soient mis-à-jour,
* scripts de post-migration (postmigr) : les scripts de post-migration (postmigr) 
  sont prévus pour être exécutés après que l'application et les familles en base
  de données aient été mis-à-jour.

Les scripts doivent être situés dans le sous-répertoire de l'application et leur
nom doit être de la forme :

* scripts de pré-migration (premigr) : `${APPNAME}_premigr_${VERSION}`
* scripts de post-migration (postmigr) : `${APPNAME}_postmigr_${VERSION}`

Remarques :

* Le script peut être écrit dans n'importe quel langage que peut
exécuter la plateforme hébergeant le contexte sur lequel il est exécuté.
Toutefois, il ne doit pas avoir d'extension, il faut donc utiliser la 
notation [shebang][wikiShebang] en première ligne de script pour indiquer le 
langage d'exécution,
* les scripts de migration sont exécutés sur les changements de VERSION
sans tenir compte des RELEASE. On ne peut donc pas exécuter de script de
migration lors d'un changement de RELEASE,
* les scripts de migration doivent être exécutables.

Lors de la mise à jour d'un module, il est possible d'exécuter des scripts pour
effectuer des opérations de migrations ou des traitements spécifiques.

Ces scripts de migration ne peuvent être utilisés que dans le cadre de **mise à
jours d'application**.  
Un module sans application ne peut pas utiliser cette mécanique de migration.

Deux types de scripts sont possibles :

* scripts de pré-migration (premigr) : les scripts de pré-migration (premigr) 
  sont prévus pour être exécutés avant que l'application et les familles en base
  de données soient mis-à-jour,
* scripts de post-migration (postmigr) : les scripts de post-migration (postmigr) 
  sont prévus pour être exécutés après que l'application et les familles en base
  de données aient été mis-à-jour.

Les scripts doivent être situés dans le sous-répertoire de l'application et leur
nom doit être de la forme :

* scripts de pré-migration (premigr) : `${APPNAME}_premigr_${VERSION}`
* scripts de post-migration (postmigr) : `${APPNAME}_postmigr_${VERSION}`

Remarques :

* Le script peut être écrit dans n'importe quel langage que peut
exécuter la plateforme hébergeant le contexte sur lequel il est exécuté.
Toutefois, il ne doit pas avoir d'extension, il faut donc utiliser la 
notation [shebang][wikiShebang] en première ligne de script pour indiquer le 
langage d'exécution,
* les scripts de migration sont exécutés sur les changements de VERSION
sans tenir compte des RELEASE. On ne peut donc pas exécuter de script de
migration lors d'un changement de RELEASE,
* les scripts de migration doivent être exécutables.

## Exécution des scripts {#core-ref:673690e2-4cdb-41c5-b035-1511aec0069e}

Le lancement de ces scripts est commandé dans le fichier `info.xml` du module
par l'utilisation d'une directive 
`<process command="programs/(pre|post)_migration ${APPNAME}" />` 
dans la section `<post-upgrade/>`.

Exemple de déclaration type pour le lancement de scripts de pré-migration et de
post-migration :

    [xml]
    <module name="FOO" version="VERSION" release="1">
      ...
      <post-upgrade>
        <process command="programs/pre_migration FOO" />
        <process command="programs/record_application FOO" />
        <process command="programs/app_post FOO U" />
        <process command="programs/post_migration FOO" />
        <process command="programs/update_catalog" />
      </post-upgrade>
    </module>

Les programmes `pre_migration` et `post_migration` exécutent tous les scripts de
"premigr" ou "postmigr" dont la version est comprise entre la version de
l'application actuellement installé, et la version de l'application mise-à-jour.

| Version sur le contexte  | Version dans le paquet     | Version du script    | Exécution  |
| ------------------------ | -------------------------- | -------------------- | ---------- |
| 1.0.0                    | 2.0.0                      |  1.1.0               |  Oui       |
| 1.0.0                    | 1.0.0                      |  1.0.0               |  Non       |
| 3.0.0                    | 2.0.0                      |  1.0.0               |  Non       |
| 1.0.0                    | 2.0.0                      |  1.0.0               |  Non       |
| 1.0.0                    | 2.0.0                      |  2.0.0               |  Oui       |

## Variables d'environnement {#core-ref:de62dd12-c82f-4856-ab17-7498b698012a}

Lorsque les scripts "premigr" et "postmigr" sont exécutés, les variables
d'environnement suivantes sont disponibles :

|       Variable      |                                                                          Valeur                                                                         |           Exemple           |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| wpub                | Le chemin d'accès au contexte sur lequel est effectué la mise-à-jour.                                                                                   | "/var/www/dynacase"         |
| httpuser            | L'UID du process httpd/Apache.                                                                                                                          | "www-data"                  |
| pgservice_core      | Le nom du service PostgreSQL d'accès à la base de données Dynacase.                                                                                     | "dynacase"                  |
| MODULE_VERSION_FROM | La version du module actuellement installée (de la forme "VERSION-RELEASE"). Cette version n'est pas forcément égale à la version de l'application | "1.2.3-4"                   |
| MODULE_VERSION_TO   | La nouvelle version du module (de la forme "VERSION-RELEASE").                                                                                          | "2.3.4-5"                   |
| WIFF_CONTEXT_NAME   | Le nom du contexte sur lequel est effectué la mise-à-jour.                                                                                              | "dynacase"                  |
| WIFF_CONTEXT_ROOT   | Le chemin d'accès au contexte sur lequel est effectué la mise-à-jour.                                                                                   | "/var/www/dynacase"         |
| WIFF_ROOT           | Le chemin d'accès au répertoire dans lequel est installé dynacase-control.                                                                              | "/var/www/dynacase-control" |


## Création de script PHP utilisant l'API de Dynacase {#core-ref:865dc8e8-a4c6-4be7-b8e1-0fd9a9fbdd9f}

Les scripts de migrations doivent pouvoir être de bas niveau comme ceux présentés
ci-dessus. Il s'exécute en ayant un minium de dépendances et permettent de faire
des mise à jour sur des parties très bas niveau de Dynacase (base de données, 
mécanisme de persistances, etc.). Toutefois, dans le cas d'une application, il
est souvent plus pratique d'utiliser les API de Dynacase soit via [les scripts][scripts_cli], soit
via les [applications actions][action].

Pour ce faire, vous pouvez utiliser l'exemple suivant :

    [bash]
    
    #!/bin/bash
    
    set -o errexit
    set -o nounset
    
    ME=$(basename "$0")
    
    function wsh_api {
      logger -t "$ME" -- "BEGIN:$@"
      "$wpub/wsh.php" "$@"
      logger -t "$ME" -- "END:$@"
    }
    
    # Pour un script CLI
    wsh_api --api=[NOM_DU_SCRIPT_CLI] --from="$MODULE_VERSION_FROM" --to="$MODULE_VERSION_TO"
    
    # Pour une action
    wsh_api --app=[NOM_APPLICATION] --action=[NOM_ACTION] --from="$MODULE_VERSION_FROM" --to="$MODULE_VERSION_TO"

Le code ci-dessus appelle une action ou un script cli en passant en paramètres
entrant les numéros de version (le passage des numéros de version est
facultatif).

**Note** : Pour comparer les numéros de version passés en paramètres, vous pouvez
utiliser la fonction [`version_compare`][phpVersionCompare].

## Comparaison de versions pour les scripts bash {#core-ref:4a003255-f158-4192-b09d-8d203ed612ba}

Il peut être nécessaire, pour un script de migration, d'effectuer des opérations
différentes en fonction de la version de départ du module. Pour cela, la
variable d'environnement `$MODULE_VERSION_FROM` peut-être utilisée pour obtenir
la version du module actuellement installé. Ensuite, pour comparer des versions
représentés sous forme de chaîne de caractères, il vous faudra utiliser la
fonction bash `versionCompare` déclarée dans le fichier bash `$wpub/libutil.sh` :

Syntaxe :

    [bash]
    versionCompare $V1 $V2

La fonction prend deux arguments, qui sont les versions à comparer, et affiche
sur STDOUT un entier : inférieur à 0 si $V1 est inférieur à $V2 supérieur à 0 si
$V1 est supérieur à $V2 égal à 0 si $V1 est égal à $V2

Il faut ensuite utiliser les opérateurs de comparaison arithmétique ("-lt",
"-gt", "-le", "-ge", "-eq", "-ne") de bash :

    [bash]
    #!/bin/bash
    
    . "$wpub/libutil.sh"
    
    CMP=$(versionCompare "$MODULE_VERSION_FROM" "1.0.0")
    if [ $CMP -lt 0 ]; then
      echo "La version de départ est inférieure à 1.0.0"
    else
      echo "La version de départ est supérieure ou égale à 1.0.0"
    fi

<!-- links -->

[scripts_cli]:        #core-ref:1566c46d-a53d-44cf-8c3f-0d0e21c0b117
[action]:             #core-ref:5f3cfda3-6293-4b3b-8b9f-616e9f9b029f
[wikiShebang]:        https://en.wikipedia.org/wiki/Shebang_(Unix) "Wikipedia : shebang"
[phpVersionCompare]:  http://us1.php.net/manual/en/function.version-compare.php "PHP : version_compare"