# manageContextCrontab {#core-ref:6aa4de16-1e51-49df-9926-7079aab0d268}

## Description {#core-ref:3933ec74-cda9-4ebf-9215-a3c1d4456af4}

Le script `manageContextCrontab` permet d'enregistrer, ou supprimer, des crontabs du contexte
dans l’ordonnanceur cron du système.

## Usage {#core-ref:eb6cebe0-ef8d-44f7-89d7-599b840842ee}

**`cmd` (obligatoire)**
:   L'opération à effectuer. L'opération peut être :
    
    `list`
    :   Afficher la liste des fichiers de crontab chargés et actifs pour le
        contexte.
    
    `register`
    :   Charger, ou mettre à jour, un fichier de définition de crontab pour le
        contexte.
    
    `unregister`
    :   Décharger un fichier de définition de crontab pour le contexte.
    
    `unregister-all`
    :   Décharger toutes les définitions de crontab pour le contexte.

`file`
:   Permet de spécifier le fichier de crontab à charger/décharger pour le
    contexte. Le contenu du fichier doit être conforme à la syntaxe des fichiers
    cron du système
    (c.f. [`man 5 crontab`][man5_crontab]).
    
    Cette option est requise pour les opérations `register` et `unregister`.

`user`
:   Permet de spécifier le nom de l'utilisateur système sous lequel sont
    chargés/déchargés les fichiers crontab. Par défaut, l'uid de l'utilisateur
    courant (utilisateur du process Apache par ex.) est utilisé.

## Limite d'usage {#core-ref:8b2c9dee-4586-4985-bc94-899c011b4be0}

N/A

<!-- links -->
[man5_crontab]: http://manpages.ubuntu.com/manpages/precise/en/man5/crontab.5.html
