# Doc::postDelete()  {#core-ref:8db8e5e0-b15e-4cfe-a891-66850df15b1e}
<div class="short-description" markdown="1">  

[Hameçon][hook] (ou hook) utilisé par la méthode [`Doc::Delete()`][Delete].
Cette méthode est appelée **après** la suppression du document.

</div>

## Description  {#core-ref:f447342b-b86b-41d8-afbb-62498ecaef34}

    [php]
    string postDelete (  )

Cette méthode permet de réaliser un post-traitement après suppression du
document.

### Avertissements  {#core-ref:81f4fce5-067b-4d64-bf3a-9bb4b2e842ae}

Le document est déjà supprimé lors de l'appel. Dans le cas d'une suppression
physique, il n'est plus en base de données.

## Liste des paramètres  {#core-ref:fcd1e2c5-2f8f-44cd-a93f-031e2c67ad39}

Aucun.

## Valeur de retour  {#core-ref:507fcfd4-5309-40a5-a6d9-1540f69deda0}

Message d'information.  Ce message est enregistré dans
l'historique du document.

## Erreurs / Exceptions  {#core-ref:5a1de851-4460-49ac-bae0-3bdd4a9aa574}

Aucune.

## Historique  {#core-ref:6441bfb8-077c-4416-b164-79e68226b77e}

Aucun.

## Exemples  {#core-ref:1b68b88e-7ddb-417e-b9c4-753d676d6a14}

Cet exemple supprime un document lié.

    [php]
    namespace My;
    use \Dcp\AttributeIdentifiers\MyFamily as MyAttributes;
    
    class MyFamily extends Dcp\Family\Document
    {
        public function postDelete() {
            $linkedId=$this->getAttributeValue(MyAttributes::sp_linkeddoc);
            if (linkedId != "")) {
                $linked=\Dcp\DocManager::getDocument(linkedId);
                if ($linked && $linked->isAlive()) {
                    return $linked->delete();
                }
            }
            return '';
        }
    }

## Notes  {#core-ref:dc75185f-6dff-4b96-8667-349fe771747f}

Aucune.

## Voir aussi  {#core-ref:35801bff-af17-4fbc-ab40-c10b55c4cf22}

*   [`Doc::postDelete()`][docpostDelete],
*   [`Doc::undelete()`][undelete].

<!-- links -->
[docstore]:         #core-ref:b8540d13-ece6-4e9e-9b72-6a56bca9da12
[docpostcreated]:   #core-ref:b8f80e6b-a374-4bf4-bc76-47290cd69c45 "Hameçon Doc::postCreated()"
[docpoststore]:     #core-ref:99520a31-0aef-4bc6-b20a-114737059d17 "Hameçon Doc::postStore()"
[docprestore]:      #core-ref:3517da95-82fe-4adb-8bc4-ef49ca55edb0 "Hameçon Doc::preStore()"
[docprecreated]:    #core-ref:e85aa9d4-5e62-4a60-9d1c-f60433301747 "Hameçon Doc::preCreated()"
[docprerefresh]:    #core-ref:580d6be1-6b6a-439b-abd7-34b26cfaf2e5 "Hameçon Doc::preRefresh()"
[docpostrefresh]:   #core-ref:9352c534-3691-41e3-b293-599db8e9a4fd "Hameçon Doc::postRefresh()"
[docpreimport]:     #core-ref:adb6ba8b-15c4-42d3-97dc-1da16c2112ae "Hameçon Doc::preImport()"
[docpostimport]:    #core-ref:9de7e922-150a-416b-b846-b6e195bf0921 "Hameçon Doc::postImport()"
[docpostDelete]:   #core-ref:8db8e5e0-b15e-4cfe-a891-66850df15b1e "Hameçon Doc::postDelete()"
[docpreDelete]:    #core-ref:dca7b2bd-fc69-4f6f-ab12-fb0de91dce8c "Hameçon Doc::preDelete()"
[undelete]:         #core-ref:e48b02c7-c684-4f71-a731-ac92064d13ae
[hook]:             http://fr.wikipedia.org/wiki/Hook_(informatique) "Définition de Hook sur wikipedia"