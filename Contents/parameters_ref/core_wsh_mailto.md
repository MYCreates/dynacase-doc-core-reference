# `CORE_WSH_MAILTO` {#core-ref:6457a9c1-8b3e-4fd1-913a-8f9e133fc7a4}

## Description {#core-ref:0c738962-838d-4b89-a008-ce5151488225}

<span class="flag from release inline">3.2.23</span> Le paramètre `CORE_WSH_MAILTO`
indique à qui doivent être envoyés les mails lors
d'[erreurs wsh en mode non interactif][core-ref:wsh_error].

*   App : `CORE`
*   Portée : Global
*   Valeur initiale : vide
*   Utilisateur : Non

## Valeurs {#core-ref:b797069c-55b3-451b-93e9-094999eaf7ee}

La valeur est une liste d'adresses mail séparées par des virgules.
Par exemple :

    john.doe@example.net, jane.doe@example.net

## Notes {#core-ref:0bc42229-9e13-411d-b2a1-5dca3ce3aa6d}

Le sujet du mail envoyé est défini par le paramètre
[`CORE_WSH_MAIL_SUBJECT`][core-ref:CORE_WSH_MAIL_SUBJECT].

<!-- links -->

[core-ref:CORE_WSH_MAIL_SUBJECT]: #core-ref:19f573cf-a1ea-4fa1-8eb8-a69c54139f9a
[core-ref:wsh_error]: #core-ref:5ada82f5-0f47-4345-b99b-e33901d9bc3a
