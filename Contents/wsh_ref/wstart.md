# wstart {#core-ref:04a8b73b-c1b3-44a9-bb19-6a712592a7f3}

## Description {#core-ref:5ba4f367-0f5f-4e3b-9349-c677fd911d41}

Le script `wstart` permet de reconfigurer certains éléments du contexte et de
sortir le contexte du mode de maintenance.

Le script `wstart` se trouve à la racine du contexte et n'est donc pas un
script d'API [`wsh`][wsh].

Par défaut, toutes les opérations sont effectués dans l'ordre suivant :

* régénération du cache de l'autoloader de classes ;
* régénération des liens symboliques des images dans le répertoire `Images` à
  la racine du contexte ;
* suppression des fichiers de cache dans le répertoire `var/cache/image` ;
* incrément du compteur de version `WVERSION` ;
* (**déprécié**) configuration du mode de connexion à la base de données ;
* régénération des styles ;
* désactivation du mode maintenance du contexte.

Chacune de ces opérations peut être exécutée manuellement en lançant `wstart`
avec les options décrites ci-dessous.

## Usage {#core-ref:8f1658c8-ef20-4398-98f6-a959da65c6e5}

`-a`
`--all`
:   Lance toutes les opérations (comportement par défaut).

`-r`
`--resetAutoloader`
:   Lance la régénération du cache d'autoloader de classes.

`-l`
`--links`
:   Lance la régénération des liens symbolique des images dans le
    sous-répertoire `Images` du contexte.

`-c`
`--clearFile`
:   Supprime les fichiers `*.png`, `*.gif`, `*.xml` et `*.src` dans le
    sous-répertoire `var/cache/image` du contexte.

`-u`
`--upgradeVersion`
:   Incrémente le compteur de version `WVERSION`.

`-b`
`--dbconnect`
:   (**déprécié**) Applique le mode de connexion spécifié par le paramètre
    applicatif `CORE_DBCONNECT`.

`-s`
`--style`
:   Lance la régénération des style (voir [`setStyle`][setStyle]).

`-m`
`--unStop`
:   Désactive le mode de maintenance.

`-v`
`--verbose`
:   Affiche les opérations qui sont effectués (par défaut seuls les messages
    d'erreurs sont affichés).

## Limite d'usage {#core-ref:2ef2df4f-696a-4847-a772-41dc779bf378}

* Plusieurs options peuvent être spécifiés afin de sélectionner plusieurs
  opérations à réaliser.
* L'ordre des options n'a pas d'effet sur l'ordre dans lequel les opérations
  sélectionnés sont exécutées : les opérations sélectionnés sont exécutées dans
  l'ordre décrit au chapitre [`Description`](description).

## Voir aussi {#core-ref:16922db1-88f2-4681-aa18-986fd6f2073e}

* [`wstop`][wstop]

<!-- links -->
[wstop]: #core-ref:f88c39a9-dc65-4443-a344-ab0ab56f4ca1
[wsh]: #core-ref:1566c46d-a53d-44cf-8c3f-0d0e21c0b117
[setStyle]: #core-ref:a2898d77-9f34-4b4c-ab50-9fb3cad6f140
[description]: #core-ref:5ba4f367-0f5f-4e3b-9349-c677fd911d41
