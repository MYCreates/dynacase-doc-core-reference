# Minuteur {#core-ref:3de1c186-e1ab-44a3-b3b1-536d2f9a7554}

Les minuteurs permettent de déclencher des actions à des dates prédéfinies. Ces
dates peuvent être statiques, ou liées à des valeurs du document qui porte le
minuteur.

C'est utile, par exemple, pour répondre à une exigence de la forme :

>   Le délai de validation par défaut est de 15 jours, mais il est modifiable
    par l'utilisateur.  
    5 jours avant la date butoir, un mail de relance est envoyé au valideur  
    À la date butoir, si le document n'est pas relu, il est passé dans l'état
    *Non validé*.

## Actions {#core-ref:ab602a0f-caea-4124-90d8-8c9d4c34971c}

Un minuteur effectue une séquence d'action. Chaque action peut être :

*   un envoi de mail
*   un changement d'état
*   l'exécution d'une méthode du document portant le minuteur
*   une combinaison des 3 types d'actions précédentes. Dans ce cas l'ordre
    d'exécution est :
    1. le changement d'état
    1. l'envoi de mail
    1. l'exécution de la méthode.

Chaque action est définie par un délai, et un nombre d'itérations.

## Séquençage des actions {#core-ref:286ba91c-4665-4d19-b6c7-a87826f9b4b5}

1.  la première itération de la première action est déclenchée à
    *[date de référence][date_de_reference] + délai*
2.  Une fois une itération d'une action effectuée,
    1.  s'il reste des itérations, l'itération suivante sera effectuée à
         à *date de référence + Σ(actions passées x itérations passées* )
    2.  s'il ne reste plus d'itération, l'action suivante sera effectuée à
         à *date de référence + Σ(actions passées x itérations passées* )
3.  Une fois toutes les itérations de toutes les actions effectuées, le minuteur
    est détaché du document.

<span class="flag nota-bene"/>Si le date de référence est dynamique, le minuteur
revient à la première itération à chaque modification de cette date de
référence. 

## Date de référence {#core-ref:386637d4-ab5b-4b3b-bf80-f2e6c226c555}

Par défaut, la date de référence d'un minuteur est sa date d'attachement au
document. Il est possible de modifier cette date de référence de 2 manières :

Date de référence dynamique
:   La date de référence peut être reliée à un attribut de type *date* ou 
    *timestamp* du document auquel est rattaché le minuteur.
    
    Dans ce cas, la date de référence est recalculée à chaque mise à jour du
    document.
    
    <span class="flag from release">3.2.18</span> Si la date reliée est vide, le
    minuteur n'est pas activé. Il reste en attente d'activation jusqu'à ce que cette
    date reliée soit valorisée. De même, si la date reliée devient vide, les 
    actions du minuteur sont inhibées.

Décalage de la date de référence
:   Il est possible d'appliquer un delta à cette date de référence, qu'elle soit
    dynamique ou non. Ce delta peut servir, par exemple, à effectuer une action,
    *N jours avant la date cible* : dans ce cas, on appliquera un delta de
    *-N jours*.
    
    Ainsi, pour répondre au besoin donné en introduction, on appliquera le
    paramétrage suivant :
    
    *   Lors de l'envoi en validation, la date butoir est demandée à
        l'utilisateur (et sa valeur par défaut est positionnée à
        *aujourd'hui + 15 jours*)
    *   La date saisie est enregistrée dans l'attribut *date_butoir_validation*
    *   Un minuteur est paramétré comme suit :
        *   date de référence dynamique : *date_butoir_validation*
        *   delta : *-5 jours*
        *   Action 1 :
            *   délai : 0
            *   occurrences : 1
            *   mail : [le modèle de mail de notre choix]
            *   changement d'état : aucun
            *   méthode : aucune
        *   Action 2 :
            *   délai : 5 jours
            *   occurrences : 1
            *   mail : aucun
            *   changement d'état : *non validé*
            *   méthode : aucune
            
    **Note** : Le délai peut aussi être négatif. Il est donc également possible
    d'appliquer le paramétrage suivant :
    
    *   Lors de l'envoi en validation, la date butoir est demandée à
        l'utilisateur (et sa valeur par défaut est positionnée à
        *aujourd'hui + 15 jours*)
    *   La date saisie est enregistrée dans l'attribut *date_butoir_validation*
    *   Un minuteur est paramétré comme suit :
        *   date de référence dynamique : *date_butoir_validation*
        *   delta : 0
        *   Action 1 :
            *   délai : *-5 jours*
            *   occurrences : 1
            *   mail : [le modèle de mail de notre choix]
            *   changement d'état : aucun
            *   méthode : aucune
        *   Action 2 :
            *   délai : 0
            *   occurrences : 1
            *   mail : aucun
            *   changement d'état : *non validé*
            *   méthode : aucune

## Historique {#core-ref:3f736d31-4f76-46c5-bb50-4ef6a7861a78}

Lors de l’exécution d'une action, une entrée d'historique est ajoutée au
document auquel est attaché le minuteur.

## Administration {#core-ref:eedae048-f3ca-42fc-9ed9-f2f2a63d0412}

Les minuteurs peuvent être administrés depuis le _centre d'administration_.
(application : DOCADMIN, action : TIMERS_ADMIN).

## Programmation {#core-ref:c261e2cc-4766-41a7-a363-c5b57c4c4544}

Les minuteurs peuvent être gérés par [programmation][doctimers]. Cette
programmation permet d'attacher ou de détacher les minuteurs associés à un
document.

## Gestion des erreurs {#core-ref:d0388efe-e19a-4208-bc22-1b192031d03c}

Lorsqu'un shell non interactif[^core-ref:shell non interactif] sort en erreur,
un mail contenant le détail de l'erreur peut être envoyé.

Ce mail est envoyé uniquement si le paramètre [`CORE_WSH_MAILTO`][core-ref:CORE_WSH_MAILTO] est non vide.

<span class="flag inline nota-bene"/> Lorsque le traitement d'un minuteur provoque une erreur,
le document concerné est à nouvau traité par les minuteurs suivants
tant que sa date de déclenchement reste valide (voir FDL_TIMERHOURLIMIT).

<!-- links -->

[core-ref:CORE_WSH_MAILTO]: #core-ref:6457a9c1-8b3e-4fd1-913a-8f9e133fc7a4
[date_de_reference]: #core-ref:386637d4-ab5b-4b3b-bf80-f2e6c226c555
[doctimers]:   #core-ref:6403d0d7-9e4c-42e9-8a07-a2256a7c43f7
