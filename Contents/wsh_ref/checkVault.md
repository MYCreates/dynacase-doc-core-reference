# checkVault {#core-ref:147bcb5f-5415-4902-8df4-26b77e508a5f}

## Description {#core-ref:f9750692-1e3d-4671-bc01-91a0e73c5963}

Le script `checkVault` permet d'identifier les fichiers du vault qui ne sont
référencés par aucun document Dynacase (fichiers orphelins), ou bien les
documents Dynacase qui référencent des fichiers qui n'existent plus dans le
vault.

Le script permet ensuite de supprimer ces fichiers orphelins.

## Usage {#core-ref:f1af54b5-a41a-4d09-847a-8bea3054ea01}

`vault` (default `FREEDOM`)
:   Le nom du vault que l'on souhaite inspecter ou modifier.

`test` 
:   Permet d'afficher les opérations qui seront effectuées sans
    modifier quoi que ce soit.

**`cmd` (obligatoire)**
:   Permet de spécifier l'opération à effectuer. L'opération peut être :
    
    `check-all`
    :   Rapporte les fichiers du vault qui ne sont référencés par aucun
        document Dynacase (fichiers orphelins), et rapporte les documents
        Dynacase qui référencent des fichiers qui n'existent plus dans le vault.
    
    `check-noref`
    :   Rapporte les fichiers du vault qui ne sont référencés par aucun
        document Dynacase (fichiers orphelins).
    
    `check-nofile`
    :   Rapporte les documents Dynacase qui référencent des fichiers qui
        n'existent plus dans le vault.
    
    `clean-unref`
    :   Plus utilisable depuis la version 
        <span class="flag from release inline">3.2.21</span>.
        Le script [`cleanVaultOrphans`][cleanVaultOrphans] permet la suppression 
        des fichiers orphelins (fichiers qui ne sont référencés par
        aucun document Dynacase).

`csv` 
:   Permet d'afficher le résultat de l'inspection sous la forme d'un fichier CSV
    avec comme séparateur le point-virgule. (pour utilisation par un outils tiers).

## Limite d'usage {#core-ref:c04842e5-cce2-422d-9848-e79885b36098}

La bonne exécution de cette commande peut nécessiter, au préalable, l'exécution
du script d'API
[`refreshVaultIndex`][refreshVaultIndex] afin de
correctement détecter les incohérences de relations entre les fichiers et les
documents Dynacase.

<!-- links -->
[refreshVaultIndex]: #core-ref:17575c41-0b44-4164-8876-c08f4c2c3eb3
[cleanVaultOrphans]:    #core-ref:8bc46f84-c5f1-40f4-981a-37a15e67a46e