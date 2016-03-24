# Classe SearchAccount {#core-ref:dc8236f9-2389-49a6-b582-bc6b332880b1}

La classe `SearchAccount` permet de réaliser facilement la recherche de 
[comptes][introAccount].

Le résultat de cette recherche peut retourner des [utilisateurs][user], [des
groupes][group] ou [des rôles][role].

## Constructeur {#core-ref:5405bf7f-da07-4455-a136-10f29c236567}

    [php]
    new SearchAccount( )

### Liste des paramètres {#core-ref:1ec0a75f-a73a-4f13-a3c0-fafded8c5d4a}

Aucuns

### Propriétés de la classe {#core-ref:6213507b-93b6-4169-9cb1-3c8ef9504a9e}

Aucunes

### Exemple {#core-ref:275173f0-b74e-4cbc-8f34-0f5204811f9b}

    [php]
    $searchAccount = new SearchAccount();
    $searchAccount->addRoleFilter('writter');
    $searchAccount->setObjectReturn($searchAccount::returnAccount);
    /**
    * @var \AccountList $accountList
    */
    $accountList = $searchAccount->search();
    /**
    * @var \Account $account
    */
    foreach ($accountList as $account) {
        $login = $account->login;
        print "$login\n";
    }

## Voir aussi {#core-ref:59a0aefd-8b96-4ea0-acbe-109d1963eaff}

* [introduction sur les comptes][introAccount],
* [chapitre sur les comptes][chapterAccount],
* [chapitre sur la recherche des utilisateurs][advancedUserSearch],
* [base de données des comptes][dataAccount].

<!-- links -->

[introAccount]:     #core-ref:f6d55bb1-7254-480e-9bfa-61f5e532ccad
[user]:             #core-ref:ecd3b9fb-f038-48ed-9eea-04b9bbeabb51
[group]:            #core-ref:e01fc125-52ef-4c48-b4c6-95ddeac23327
[role]:             #core-ref:6017a086-3211-485f-b68a-b93850953065
[chapterAccount]:   #core-ref:2bd98eec-5b03-4af0-b9d6-1bbf78fe9733
[dataAccount]:      #core-ref:6d5684f4-73e8-431c-8b2b-6224a9e6b074
[advancedUserSearch]:   #core-ref:77224212-a4f3-4159-8a8a-c11c6f430f61