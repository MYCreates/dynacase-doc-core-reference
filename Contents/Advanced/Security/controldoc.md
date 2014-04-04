# Vérification des droits pour accéder à un document {#core-ref:5f7ae923-fff6-405d-a394-d1ad828ced9e}

La consultation des documents est contrôlée par les [profils de
documents][acldoc].

Le contrôle d'accès au document est fait par les interfaces de haut-niveau. La
[classe Doc][classdoc] et notamment la méthode
[`DocManager::getDocument()`][getdocument] ne vérifie pas les accès. Ces
vérifications doivent être fait par les actions avec la méthode
[`Doc::hasPermission()`][docctrl].

Le schéma suivant décrit les contrôles de l'action [FDL:OPENDOC][opendoc] et
précise la cinématique de l'[accès à un document][viewdocreq].

![Contrôle de l'affichage d'un document](advanced/controlviewdoc.png)

<!-- links -->
[profilrules]:  #core-ref:fc37efd3-6254-4bc8-8f8d-7f867c852b67 "Propagation de profilage"
[accountintro]: #core-ref:2bd98eec-5b03-4af0-b9d6-1bbf78fe9733 "Utilisateurs, groupes et rôles" 
[actionacl]:    #core-ref:a98b72ea-c063-4907-abc4-e5171ab55e59 "Les droits applicatifs"
[acldoc]:       #core-ref:a99dcc5f-f42f-4574-bbfa-d7bb0573c95d "Paramétrage des droits documentaires"
[execreq]:      #core-ref:009929b6-8ffa-4306-add7-14cbdb41e6b4
[viewdocreq]:   #core-ref:76241972-720a-464f-a43b-04c9884b8101
[classdoc]:     #core-ref:1d557fb4-4eca-4ab8-a334-974fe563ddd2
[docctrl]:      #core-ref:96f8ce9d-7bfb-4027-8106-10cbe8ee6a1a
[opendoc]:      #core-ref:f9e68fa7-01b7-4903-9718-744271d63112
[exportfile]:   #core-ref:0fc93676-888f-497e-b9bc-28cdbead52df
[dbvault]:      #core-ref:7c41b8d8-5ace-489c-886f-a6500c717423
[getdocument]:  #core-ref:dfa0762f-6ff3-4349-bd21-6442740d9dcc