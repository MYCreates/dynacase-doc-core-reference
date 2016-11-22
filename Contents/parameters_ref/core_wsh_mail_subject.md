# `CORE_WSH_MAIL_SUBJECT` {#core-ref:19f573cf-a1ea-4fa1-8eb8-a69c54139f9a}

## Description {#core-ref:80b62f8e-9825-448d-98cd-284a6f7b8ea3}

<span class="flag from release inline">3.2.23</span> Le paramètre `CORE_WSH_MAIL_SUBJECT`
indique le sujet des mails envoyés lors
d'[erreurs wsh en mode non interactif][core-ref:wsh_error].

*   App : `CORE`
*   Portée : Global
*   Valeur initiale : vide
*   Utilisateur : Non

## Valeurs {#core-ref:4f947ede-5fe5-42d1-ad7a-513f1a8a21d1}

La valeur est une chaîne de caractère dans laquelle les séquences suivantes sont remplacées :

`%h`
:   Le nom d'hôte de la machine (tel que retourné par [`php_uname("n")`][php.net:php_uname]).

`%c`
:   La valeur du paramètre [`CORE_CLIENT`][core-ref:CORE_CLIENT].

`%m`
:   La première ligne du message d'erreur.

`%%`
:   le caractère `%`

Toute autre séquence de la forme `%<texte>` sera remplacée par `<texte>`.

## Notes {#core-ref:d7611cb2-7809-43c7-84ce-d0ca8227be3c}

ce paramètre n'est pris en compte que si la valeur du paramètre
[`CORE_WSH_MAILTO`][core-ref:CORE_WSH_MAILTO] est non vide.

<!-- links -->

[core-ref:CORE_CLIENT]: #core-ref:4b382037-636d-4908-9385-4d036794d98a
[core-ref:CORE_WSH_MAILTO]: #core-ref:6457a9c1-8b3e-4fd1-913a-8f9e133fc7a4
[core-ref:wsh_error]: #core-ref:5ada82f5-0f47-4345-b99b-e33901d9bc3a
[php.net:php_uname]: http://php.net/manual/function.php-uname.php
