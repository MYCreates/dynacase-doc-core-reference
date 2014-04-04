# Dcp\DocManager\Cache::setHandler()   {#core-ref:4c6ec4b0-363d-4230-8086-0bbdd82f2e1d}

<div class="short-description">
Modifie le comportement du cache.
</div>


## Description   {#core-ref:cdb1855f-b4ff-4130-8043-23ff13e96772}

    [php]
    static void Dcp\DocManager\Cache::setHandler ( Dcp\DocManager\CacheInterface $cache)

Cette méthode permet de changer le comportement du cache de document.

L'interface `CacheInterface` est composée des méthodes suivantes :

Ces méthodes ne déclenchent pas d'exception.

*   `&$mixed ::get(scalar $key);`
:    Récupère un objet du cache (retourne `null` si pas trouvé)
*   `bool  ::set(scalar $key, mixed &$item);`
:    Ajoute au cache ou met à jour le cache (retourne `true` en cas de succès)
*   `bool  ::unSet(scalar $key);`
:    Supprime un objet du cache (retourne `true` en cas de succès)
*   `bool  ::clear();`
:    Supprime tous les objets du cache (retourne `true` en cas de succès)
*   `bool ::getKeys(array &$keys);`
:    Récupère la liste des clefs du cache (retourne `true` en cas de succès)
*   `bool ::exists(scalar $key);`
:    Test la présence d'un objet dans le cache (retourne `true` si trouvé)


### Avertissements   {#core-ref:7c0ae390-8176-4d33-93f2-07913b4021b2}

Aucuns.

## Liste des paramètres   {#core-ref:6f75f13c-8c38-4072-9a61-b934ebc66bb9}
 
(Dcp\DocManager\CacheInterface) `cache`
:   Objet de gestion de cache.


## Valeur de retour  {#core-ref:fe8d081f-b011-46b7-8bf0-d49373cb9e70}

Aucune

## Erreurs / Exceptions   {#core-ref:89cea671-eb28-47c8-87f8-2f739134200d}

Aucunes.



## Historique  {#core-ref:10e3900a-20c9-446e-af69-472db5b7a046}

Aucun.

## Exemples   {#core-ref:ed745671-7159-4e55-9325-68222ffa220c}

    [php]
    class MyCache implements Dcp\DocManager\CacheInterface
    {
        // implémentation de l'interface ici
    }
    
    $myCacheObject=new MyCache();
    Dcp\DocManager::cache()->setHandler($myCacheObject);

## Notes   {#core-ref:ec2c2e66-a0de-457a-8eff-dcaaca184aef}

Aucunes.

## Voir aussi   {#core-ref:2e297436-6f5a-4a57-b547-d41f0ca831de}


*   [`Dcp\DocManager::getDocument()`][getdocument]
*   [`Dcp\DocManager::addDocument()`][addincache]

<!-- links -->
[getdocument]:      #core-ref:dfa0762f-6ff3-4349-bd21-6442740d9dcc
[addincache]:       #core-ref:15d6a036-3b6e-4dbd-a0fe-361b925e6186
