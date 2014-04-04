# Description des groupes d'utilisateurs {#core-ref:d3a9acde-f4fa-4a0b-8acc-1303f8e6b17e}

## Caractéristiques des groupes d'utilisateurs {#core-ref:1e5d9cf4-2850-49b0-b729-acd7959f65a6}

Les principaux attributs de la famille _Groupe d'utilisateurs_ sont :

Nom `grp_name`
:   Dénomination du groupe.

Identifiant `us_login` 
:   Identifiant système.
    
    Il doit être unique sur l'ensemble des comptes (utilisateurs, rôles et
    groupe).
    
    **Note** : L'identifiant doit toujours être en minuscules. S'il comporte
    des majuscules elle seront transformées en minuscules lors de
    l'enregistrement.

Mail `grp_mail`
:   adresse mail du groupe.
    
    Elle est constituée de la concaténation des mails des membres du groupe.

Sans adresse mail de groupe `grp_hasmail`
:   Permet de désactiver la génération du mail.
    
    Les valeurs possibles sont :
    
    *   **`yes` (valeur par défaut)**: génère l'adresse du groupe.
    *    `no` : ne génère pas l'adresse du groupe.
    
    Cette option peut être intéressante pour les groupes possédant un très grand
    nombre de membres. En effet, dans ce cas, le temps de calcul et notamment le
    temps d'écriture en base sont élevés.  
    En contrepartie, il sera impossible d'envoyer un mail à ce groupe.

Rôles associés `grp_roles` 
:   Tableau indiquant les rôles joués par les utilisateurs membres du groupes.
    
    Les rôles sont des éléments déterminants pour la mise en place des [droits]
    [profilage]

Fonction `grp_type`
:   Permet de caractériser le groupe

Identifiant système `us_whatid`
:   Identifiant numérique système unique.
    
    Cet identifiant est donné par le système lors de l'enregistrement. Il sert
    d'identifiant pour l'objet système associé au document. Voir
    [Account][class_account].

## Création d'un groupe {#core-ref:6cb78c8d-9563-4abc-9f28-5c4f71da4f44}

Il est possible de créer manuellement un groupe en créant un document de la
famille `IGROUP` depuis l'interface, à condition de bénéficier des privilèges
suffisants.

Par programmation il suffit de créer un document de la famille groupe `IGROUP`.

    [php]
    $dg = \Dcp\DocManager::createDocument("IGROUP");
    if ($dg) {
        $dg->setValue("us_login","driver");
        $dg->setValue("grp_name","Conducteurs");
        
        $err = $dg->store();
        
        if (!$err) {
           print "nouveau groupe n°".$dg->getValue("us_whatid"); // affichage de l'identifiant numérique système          
        } else {
           print "erreur:$err";
        }
    }


La méthode _IGROUP::getAccount() permet de récupérer l'objet système "Account".
Voir le paragraphe [Account][class_account]<span class="fixme" data-assignedto="nobody">Ajouter le lien vers le chapitre de la classe Account</span>.

## Ajout d'un groupe à un utilisateur {#core-ref:57b66262-5239-4a09-831c-423cf39a826d}

L'ajout direct d'un groupe à un utilisateur se fait depuis le groupe au moyen de
la méthode `_IGROUP::insertDocument()`.

    [php]
    $g = \Dcp\DocManager::getDocument("GADMIN");
    $u = \Dcp\DocManager::getDocument(1075); // 1075 est la référence documentaire de l'utilisateur
    if ($g && $u &&  $g->isAlive() && $u->isAlive() ) {
        printf("ajout de l'utilisateur %s [%d] au groupe %s [%d]\n",
             $u->getTitle(),$u->id,$g->getTitle(),$g->id);
        printf("Liste des groupes avant\n");
        print_r($u->getAllUserGroups());
      
        $err = $g->insertDocument($u->initid);
        if ($err) {
          print "Error:$err\n";
        } else {
          printf("Liste des groupes après\n");
          print_r($u->getAllUserGroups());
        }
    }

<!-- links -->
[profilage]: #core-ref:ce576351-dbe6-45d1-8097-f9573502b651
[class_account]:    #core-ref:68c93fb2-088c-435a-b4ac-e1b94095d0c9