# `CORE_NOTIFY_SENDMAIL` {#core-ref:acafeba4-2532-4511-96cf-aaf197c0c701}

## Description {#core-ref:30d6c956-b6cc-4a31-b3ca-e4a068a3547d}

<span class="flag from release inline">3.2.23</span> Le paramètre `CORE_NOTIFY_SENDMAIL`
indique dans quels cas une notification doit être affichée à l'utilisateur suite à l'envoi d'un mail.

*   App : `CORE`
*   Portée : Global
*   Valeur initiale : `always`
*   Utilisateur : Non

## Valeurs {#core-ref:b089325e-d7d8-4b19-b150-f02d2cae4967}

Les valeurs valides sont

`always`
:   une notification est affichée à l'utilisateur à chaque envoi de mail

`errors only`
:   une notification est affichée lorsqu'un mail n'a pas pu être envoyé

`never`
:   aucune notification n'est affichée à l'utilisateur lors des envois de mail

## Notes {#core-ref:7f3baf5d-b6e6-4f5b-8ecc-95badfcf6a1f}

Ce paramètre est pris en compte lors de l'envoi de mails par la plate-forme.

Lors des envois de mail par du code, ce comportement peut être contourné.

<!-- links -->
[core_urlindex]: #core-ref:9081464e-dfc9-4836-8577-cfa59829eaa0
[alink]:         #core-ref:aaaa5d78-0982-4c3e-a8ed-a125c49572a8
[core-ref:mailtemplate]: #core-ref:8723b1aa-10d3-4316-af6b-071f4d59ceee
