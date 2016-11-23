# Processus {#core-ref:4a65995d-a61d-4325-89e2-1a9ce15f76e8}

La famille `Processus` (nom logique `EXEC`) permet de définir des tâches/
traitements à exécuter de manière récurrente.

Le traitement est soit :

-   une [actions][core-ref:action],
-   un [script CLI][core-ref:script].

## Paramétrage {#core-ref:a269c14f-e5c8-45f1-a874-882221de7c3d}

![ Formulaire de création de processus ](advanced/processus/processus.png)

Dans cette interface, vous pouvez définir les différentes options de lancement 
de l'action ou du script, ses références et ses paramètres d'exécutions.

De plus, il est possible de définir une première date d'exécution et une 
périodicité.

En outre, il est aussi possible de ne pas indiquer de date d'exécution et
d'exécuter manuellement la tâche.

**Attention** : Si on exécute manuellement une tâche qui a une date automatique
d'exécution alors celle-ci ne s'exécute plus automatiquement, pour relancer
l'exécution automatique il faut éditer et sauvegarder le document processus.

## Limites {#core-ref:06a0a7a9-651e-4d64-8e95-57f5f84b9ee5}

Les limites sont les suivantes :

-   la périodicité ne peut être inférieure à 5 minutes,
-   le calcul de la périodicité prend en compte la fin de l'exécution de la tâche.
    Par exemple, si on définit une tâche démarrant le 31/12 à 00h00, ayant une 
    périodicité de 1 jour et durant 2H00. Cette tâche s'exécute le 31/12 à 00H00, 
    le 01/01 à 02h00, le 02/01 à 04h00 car le calcul de la périodicité de 1 jour ne
    commence qu'après la fin de la tâche.

## Gestion des erreurs {#core-ref:24c23619-519d-42e2-9437-c21869c3363c}

Lorsqu'un shell non interactif[^core-ref:shell non interactif] sort en erreur,
un mail contenant le détail de l'erreur peut être envoyé.

Ce mail est envoyé uniquement si le paramètre [`CORE_WSH_MAILTO`][core-ref:CORE_WSH_MAILTO] est non vide.

<!-- links -->

[core-ref:CORE_WSH_MAILTO]: #core-ref:6457a9c1-8b3e-4fd1-913a-8f9e133fc7a4
[core-ref:action]:   #core-ref:e67d8aeb-939c-46e3-9be8-6fc3ba75ebc2
[core-ref:script]:   #core-ref:4df1314f-9fdd-4a7f-af37-a18cc39f3505