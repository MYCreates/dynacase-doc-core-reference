# Autoloader {#core-ref:aa4be34f-3a06-4769-8bb6-fcdc002fd342}

Le mécanisme d'autoloader de Dynacase parcourt tous les fichiers PHP (extension
`.php`) du contexte pour indexer toutes les classes définies dans ces fichiers.

La liste des classes, avec leur fichier source d'inclusion, est alors stockée
dans le fichier `.autoloader.cache` à la racine du contexte Dynacase.

Le fichier de cache `.autoloader.cache` est automatiquement re-généré lorsqu'il
est inexistant (et donc lors du premier appel à l'autoloader faisant suite à la
suppression de ce fichier) ou qu'une classe demandée n'est pas trouvée dans le
cache. Suite à cette re-génération, si la classe demandée n'est toujours pas
présente dans le cache, alors une exception est levée indiquant que la classe
demandée n'existe pas.

De plus, l'autoloader est mis à jour à chaque déploiement d'une application.

Une même classe PHP (la comparaison est insensible à la casse) ne peut être
livrée par deux fichiers PHP distincts, et une exception est levée lorsque deux
fichiers PHP distincts définissant une même classe PHP sont trouvés.

## Utilisation {#core-ref:be48bd77-ae91-4b41-8fe8-afd38561a9e8}

### Ajout d'une nouvelle classe {#core-ref:4065d4f8-5cf1-4cbf-8fc6-38bacdbefa5a}

Pour ajouter une nouvelle classe, il suffit de livrer le fichier PHP qui définit
cette classe et d'utiliser normalement cette classe.

L'autoloader lancera automatiquement une régénération du cache lors de la
première utilisation de cette classe.

### Déplacement d'une classe {#core-ref:5cc70a16-2cc4-4f37-95e7-209c59b6f0f2}

Pour déplacer une classe d'un fichier PHP dans un nouveau fichier PHP (en
conservant le nom de la classe), il faut déplacer la classe dans le nouveau
fichier et supprimer le fichier de cache `.autoloader.cache` ce qui a pour
effet de régénérer ce dernier lors de la prochaine utilisation d'une classe.

### Suppression d'une classe {#core-ref:b0ad4ec4-8ce0-4cf2-9269-68e081119af0}

Pour supprimer une classe d'un fichier PHP il faut supprimer la classe de son
fichier et supprimer le fichier de cache `.autoloader.cache` ce qui a pour
effet de régénérer ce dernier lors de la prochaine utilisation d'une classe.

### Charger et activer manuellement l'autoloader Dynacase {#core-ref:ac9bb742-5eea-4748-a4b5-93b99fb5bdc8}

Par défaut, l'autoloader est chargé et actif sur tout code PHP exécuté par
Dynacase ([Action][action], [scripts CLI][scripts_cli], etc.).

Cependant, dans le cas où vous voulez exécuter un code PHP tiers (non intégré
comme [Action][action] ou [scripts CLI][scripts_cli]) qui utilise des
classes Dynacase, il faut inclure le fichier `WHAT/autoload.php` afin de charger
et activer l'autoloader Dynacase :

    [php]
    require_once 'WHAT/autoload.php';

## Personnalisation {#core-ref:e23eed6c-cbbd-4332-9c24-7695e1290e49}

### Exclusion {#core-ref:ff5bfc71-0548-4186-8b8e-48c7e5bf7fc8}

Par défaut, l'autoloader indexe tous les fichiers PHP du contexte.

Pour exclure des répertoires (ou des fichiers) de l'indexation de l'autoloader,
il faut déclarer ces éléments dans un fichier d'autoloader-ignore.

Les fichiers d'autoloader-ignore se trouvent dans le répertoire
`.autoloader-ignore.d` situé à la racine du contexte et contiennent une liste de
motifs (au format [shell glob][fnmatch]) des éléments à ignorer.

Format :

*   Les lignes commençant par un `#` (dièse) sont des commentaires et sont
    ignorés ;
    
*   Les lignes vides ou ne contenant que des espaces sont ignorés ;
    
*   Les espaces en début et en fin de motif sont conservés et interprétés comme
    tels lors de la correspondance avec les fichiers.
    
    Ainsi, `MYAPP/lib/* ` correspond aux chemins de fichiers qui commencent par
    `MYAPP/lib` et qui se terminent par un espace ` `).
    
*   Les motifs (au format [shell glob][fnmatch]) sont appliqués sur les chemins
    des fichiers PHP relatifs à la racine du contexte Dynacase (i.e.
    `MYAPP/lib/foo.php`).

Exemple de fichier `.autoloader-ignore.d/myapp` :

    # Ceci est un commentaire
    
    # Ne pas indexer les fichiers PHP sous `MYAPP/data`
    MYAPP/data/*
    
    # Idem. pour `MYAPP/lib`
    MYAPP/lib/*

### Prévention des doublons de classes {#core-ref:647f075f-0750-47cf-94fd-4445a6ac64df}

Pour se prémunir d'erreurs de doublons de classes (une même classe PHP livrée
par deux fichiers PHP distinct), vous pouvez utiliser le programme
`programs/check_autoloader` dans les phases de `<post-install/>` et
`<post-upgrade/>` (du `info.xml` du module) afin de vérifier qu'il n'y a pas de
doublons de classes.

Exemple d'utilisation dans `info.xml` :

    <module name="foo" version="1.2.3" release="4">
        [...]
        
        <post-upgrade>
    
            <process command="programs/check_autoloader">
                <label lang="en">Check autoloader</label>
            </process>
    
            [...]
        </post-upgrade>
    
    </module>


## Chemins d'inclusion (include_path) {#core-ref:c4f7a45f-9cb2-45e1-a22d-20880fa9e5d1}

Le paramètre [include_path][include_path] est défini en ajoutant le
répertoire d'installation.

<span class="flag from release inline">3.2.21</span>Le répertoire d'installation
est prioritaire aux autres chemins définis par votre configuration PHP. Les
chemins sont :

1.   `DEFAULT_PUBDIR`
2.   `DEFAULT_PUBDIR`/WHAT
3.   _Include path configuré par PHP_



<!-- links -->
[fnmatch]: http://docs.php.net/manual/en/function.fnmatch.php "php.net : fonction fnmatch"
[action]: #core-ref:e67d8aeb-939c-46e3-9be8-6fc3ba75ebc2
[scripts_cli]: #core-ref:1566c46d-a53d-44cf-8c3f-0d0e21c0b117
[include_path]: http://php.net/manual/fr/ini.core.php#ini.include-path "php.net : include_path"