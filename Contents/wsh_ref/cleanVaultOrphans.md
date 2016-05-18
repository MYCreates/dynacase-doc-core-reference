# cleanVaultOrphans {#core-ref:8bc46f84-c5f1-40f4-981a-37a15e67a46e}

<span class="flag from release">3.2.18</span>

## Description {#core-ref:0530ab29-aa8c-42a6-a09b-5a5dbd9e0b57}

Le script `cleanVaultOrphans` permet d'analyser le taux d'occupation du vault
(fichiers utilisés, fichier orphelins et fichiers manquants) et de supprimer
les fichiers orphelins.

## Usage {#core-ref:d6a6e9df-2dc2-49ef-8214-4bf08896d2f9}

`analyze` 
:   Permet d'analyser le taux d'occupation du vault et des fichiers orphelins :
    fichiers présents en base de données mais non référencés par un document.

`missing-files` (nécessite `--analyze`)
:   En conjonction avec `--analyze`, permet d'analyser les fichiers manquants :
    fichiers présents en base de données mais non présents sur le système de
    fichiers du vault.

`clean`
:   Permet de nettoyer les fichiers orphelins :
    
    * Suppression de la référence en base de données.
    * Suppression du fichier correspondant sur le système de fichiers du vault.
    
    <span class="flag inline nota-bene"/> Cette opération est irréversible. Les
    fichiers sont définitivement supprimés.


Exemple : 

    ./wsh.php --api=cleanVaultOrphans --analyze
    * Analyzing... Done.
    
    Analyze
    -------
    
    All:
        count = 418635
        size  = 207835025933 (194 GB)
        
    Used:
        count = 310387
        size  = 177660324178 (165 GB) (85.48%)
        
    Orphan:
        count = 108248
        size  = 30174701755 (28 GB) (14.52%)


## Limite d'usage {#core-ref:2c91141b-d0dc-4908-aaa2-644a964dccd8}

N/A

<!-- link -->
