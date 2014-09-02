# Description XML de migration {#core-ref:3c6a36a9-3f2e-4beb-8742-5cc16efda89e}

## Principe {#core-ref:0fd641f7-7498-4e0a-9cc0-933bc674b939}

Le fichier de migration permet de spécifier des actions qui sont composés de:

- une ou plusieurs conditions;
- un ou plusieurs processus contenant les opérations de migration à effectuer si
  les conditions sont vraies;
- une ou plusieurs vérifications.

Les actions sont évalués à chaque mise à jour, indépendamment des versions du
module ou de l'application mise à jour (au contraire du mécanisme des [scripts
de migration][migr_scripts]).

## Format fichier de migration {#core-ref:54423a1e-a494-468d-ab90-7b816c749c1c}

Le format d'un fichier de migration est fournit par la définition XSD
[`migration_1.0.xsd`][migration_xsd] ou l'URN `urn:anakeen:dcp:migration/1.0`.

Exemple d'un fichier de migration :

    [xml]
    <?xml version="1.0" encoding="utf8"?>
    <migration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:anakeen:dcp:migration/1.0">
      <action>
        <condition>
          <!-- Migrer si des produits Foo ont le prix 100 -->
          <sql-assert-empty><![CDATA[
    SELECT id FROM family.product WHERE title = 'Foo' AND price = '100';
    ]]></sql-assert-empty>
        </condition>
        <process>
          <!-- Changer le prix des produits Foo à 200 -->
          <wsh title="Set price of Foo to 200" api="setProductPrice" product="Foo" price="200" />
        </process>
        <check>
          <!-- Vérifier qu'il ne reste plus de produit Foo à 100 -->
          <sql-assert-empty><![CDATA[
    SELECT id FROM family.product WHERE title = 'Foo' AND price = '100';
    ]]></sql-assert-empty>
        </check>
      </action>
    </migration>

### Préambule et élément racine {#core-ref:8ac147ef-6cc2-4cbf-a436-3b3ed9a1520e}

Le préambule et le nœud racine sont les suivants :

    [xml]
    <?xml version="1.0" encoding="utf8"?>
    <migration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:anakeen:dcp:migration/1.0">

#### Élément `<action/>` {#core-ref:bd0e1a39-b9e5-44c9-bd58-c8f8ef70fdc4}

L'élément `<action />` permet de déclarer une action de migration qui contient
des éléments :

- [`<condition />`][condition];
- [`<process />`][process];
- [`<check />`][check].

#### Élément `<condition />` {#core-ref:85f4eb52-6caa-44a7-8647-64bcd471c6e9}

L'élément `<condition />` permet de spécifier des conditions qui si elles sont
vraies déclenchent l'exécution des éléments `<process />`.

Les propriétés de l'élément `<condition />` sont:

- `ol`: Permet de spécifier l'opérateur booléen, `and` ou `or`, de composition
    de la condition. Par défaut l'opérateur est `and`.

Exemple :

    [xml]
    <condition ol="or">
      […]
    </condition>

##### Condition `<sql-assert-true />` {#core-ref:f7224237-cffb-4b74-ae9b-045aaa2a1cf7}

La condition `<sql-assert-true />` permet d'exécuter une requête SQL dans la
base Dynacase. La requête SQL est spécifiée dans la valeur textuelle de
l'élément.

La condition `<sql-assert-false />` est vraie si la requête SQL retourne un
tuple avec une valeur booléenne `TRUE`.

Exemple :

    [xml]
    <sql-assert-true><![CDATA[
    SELECT attributeismultiple('product', 'stores');
    ]]></sql-assert-true>

##### Condition `<sql-assert-false />` {#core-ref:262f20aa-42a1-4509-b9d7-eb721da927fc}

La condition `<sql-assert-false />` permet d'exécuter une requête SQL dans la
base Dynacase. La requête SQL est spécifiée dans la valeur textuelle de
l'élément.

La condition `<sql-assert-false />` est vraie si la requête SQL retourne un
tuple avec une valeur booléenne `FALSE`.

Exemple :

    [xml]
    <sql-assert-false><![CDATA[
    SELECT attributeismultiple('product', 'name');
    ]]></sql-assert-false>

##### Condition `<sql-assert-empty />` {#core-ref:f73e0b0e-3857-45d5-bd28-5f081292c225}

La condition `<sql-assert-empty />` permet d'exécuter une requête SQL dans la
base Dynacase. La requête SQL est spécifiée dans la valeur textuelle de
l'élément.

La condition `<sql-assert-empty />` est vraie si la requête SQL ne retourne
aucun tuple.

Exemple :

   [xml]
   <!-- No products should have a price > 500 -->
   <sql-assert-empty><![CDATA[
   SELECT id FROM family.product WHERE price > 500;
   ]]></sql-assert-empty>

##### Condition `<sql-assert-not-empty />` {#core-ref:c7465871-8356-4873-a87d-3e6d887cabc5}

La condition `<sql-assert-not-empty />` permet d'exécuter une requête SQL dans
la base Dynacase. La requête SQL est spécifiée dans la valeur textuelle de
l'élément.

La condition `<sql-assert-not-empty />` est vraie si la requête SQL retourne
au moins un tuple.

Exemple :

    [xml]
    <!-- There should be at least one product with a price between 100 and 200 -->
    <sql-assert-not-empty><![CDATA[
    SELECT id FROM family.product WHERE price >= 100 AND price < 200;
    ]]></sql-assert-not-empty>

##### Condition `<php-assert-true />` {#core-ref:74b2d47e-6409-4084-8e2d-3d348fea5dfa}

La condition `<php-assert-true />` permet d'exécuter une méthode de document
Dynacase via la méthode `Doc::applyMethod()`.

Les propriétés de la condition `<php-assert-true />` sont :

- `method`(requis): Le nom de la méthode à appeler.

- `loadcontext`: `true` ou `false`. Permet de spécifier si un contexte
    d'exécution Dynacase doit être initialisé. Par défaut `loadcontext` vaut
    `true`.

La condition `<php-assert-true />` est vraie si la méthode exécutée via
`Doc::applyMethod()` retourne un booléen `TRUE`.

Exemple :

    [xml]
    <php-assert-true method="Product::checkEverythingIsFine()" loadcontext="true" />

##### Condition `<php-assert-false />` {#core-ref:98a20d40-445a-4315-868d-7dc75e8f7074}

La condition `<php-assert-false />` permet d'exécuter une méthode de document
Dynacase via la méthode `Doc::applyMethod()`.

Les propriétés de la condition `<php-assert-false />` sont :

- `method`(requis): Le nom de la méthode à appeler.

- `loadcontext`: `true` ou `false`. Permet de spécifier si un contexte
    d'exécution Dynacase doit être initialisé. Par défaut `loadcontext` vaut
    `true`.

La condition `<php-assert-false />` est vraie si la méthode exécutée via
`Doc::applyMethod()` retourne un booléen `FALSE`.

Exemple :

    [xml]
    <php-assert-false method="Product::somethingIsWrong()" loadcontext="true" />

##### Condition `<php-assert-code-return-true />` {#core-ref:e6e8c74a-83be-4e12-b2b8-0212d41f2670}

La condition `<php-assert-code-return-true />` permet d'évaluer du code PHP. Le
code PHP est spécifié dans la valeur textuelle de l'élément.

Les propriétés de la condition `<php-assert-code-return-true />` sont :

- `loadcontext`: `true` ou `false`. Permet de spécifier si un contexte
    d'exécution Dynacase doit être initialisé. Par défaut `loadcontext` vaut
    `true`.

La condition `<php-assert-code-return-true />` est vraie si le code retourne un
booléen `TRUE` (appel `return true;`).

Exemple :

   [xml]
   <php-assert-code-return-true loadcontext="true"><![CDATA[
   if (Product::everyThingIsAlright()) { {
     return true;
   }
   return false;
   ]]></php-assert-code-return-true>

##### Condition `<php-assert-code-return-false />` {#core-ref:0178630d-4a66-475f-aedd-96950d610307}

La condition `<php-assert-code-return-false />` permet d'évaluer du code PHP. Le
code PHP est spécifié dans la valeur textuelle de l'élément.

Les propriétés de la condition `<php-assert-code-return-false />` sont :

- `loadcontext`: `true` ou `false`. Permet de spécifier si un contexte
    d'exécution Dynacase doit être initialisé. Par défaut `loadcontext` vaut
    `true`.

La condition `<php-assert-code-return-false />` est vraie si le code retourne
un booléne `FALSE` (appel `return false;`).

Exemple :

   [xml]
   <php-assert-code-return-false loadcontext="true"><![CDATA[
   return Product::hasInconsistencies();
   ]]></php-assert-code-return-true>

#### Élément `<process />` {#core-ref:1d82579e-2551-482e-b074-8d58a21c4524}

Les éléments `<process />` permettent de spécifier les opérations de migration.

##### Process `<sql-query />` {#core-ref:69daa121-f642-4132-8cdc-28ce0ed663ca}

Le process `<sql-query />` permet d'exécuter une requête SQL dans la base
Dynacase. La requête SQL est spécifiée dans la valeur textuelle de l'élément ou
sous la forme d'un fichier référencé par la propriété `file`.

Les propriétés du proccess `<sql-query />` sont :

- `file`: Le chemin d'accès (relatif à la racine du contexte) d'un fichier
    contenant les instructions SQL à exécuter. Si `file` est spécifié, alors la
    valeur textuelle de l'élément est ignorée, et c'est le contenu de `file` qui
    est utilisé.

- `stop-on-error`: `true` ou `false`. Permet de spécifier si l'exécution des
    autres processus est stoppée si le process courant retourne une erreur. Par
    défaut `stop-on-error` vaut `true`.

Exemple :

    [xml]
    <sql-query stop-on-error="true"><![CDATA[
    UPDATE family.product SET price = price - 20 WHERE price > 500;
    ]]></sql-query>
    
    <sql-query stop-on-error="true" file="FOO/changePrices.sql" />

##### Process `<wsh />` {#core-ref:efb26022-63cd-4dfd-a131-18a8ab465618}

Le process `<wsh />` permet d'exécuter un script d'[API Dynacase][wsh_ref] par
un appel à [`wsh.php`][wsh_cli].

Les propriétés du process `<wsh />` sont :

- `stop-on-error`: `true` ou `false`. Permet de spécifier si l'exécution des
    autres processus est stoppée si le process courant retourne une erreur. Par
    défaut `stop-on-error` vaut `true`.

- Toutes les autres propriétés de l'élément sont passées comme arguments de
    [`wsh.php`][wsh_cli] sous la forme `--<propName>=<propValue>`.

Exemple :

    [xml]
    <!-- run ./wsh.php --api=importDocuments --file=FOO/import.ods --strict=yes -->
    <wsh stop-on-error="true" api="importDocuments" file="FOO/import.ods" strict="yes" />
    
    <!-- run ./wsh.php --app=FOO --action=updatePrices --product=Foo -->
    <wsh stop-on-error="true" app="FOO" action="updatePrices" product="Foo" />

##### Process `<bash />` {#core-ref:dbb1b395-c008-40fa-9876-ddee7397769d}

Le process `<bash />` permet d'exécuter du code Bash. Le code Bash est spécifié
dans la valeur textuelle de l'élément.

Les propriétés du process `<bash />` sont :

- `stop-on-error`: `true` ou `false`. Permet de spécifier si l'exécution des
    autres processus est stoppée si le process courant retourne une erreur. Par
    défaut `stop-on-error` vaut `true`.

Exemple :

    [xml]
    <bash stop-on-error="true"><![CDATA[
    cd FOO
    if [ $? -ne 0 ]; then
        echo "Could not change directory to 'FOO'."
        exit 1
    fi
    unzip new-products.zip
    if [ $? -ne 0 ]; then
        echo "Error unzipping 'new-products.zip' in '$PWD'."
        exit 2
    fi
    exit 0
    ]]>

##### Process `<php />` {#core-ref:16c2d8b9-727e-4dab-bf25-a32fc3e3afd7}

Le process `<php />` permet d'exécuter une méthode de document Dynacase via la
méthode `Doc::applyMethod()`.

Les propriétés de la condition `<php-assert-true />` sont :

- `method`(requis): Le nom de la méthode à appeler.

- `loadcontext`: `true` ou `false`. Permet de spécifier si un contexte
    d'exécution Dynacase doit être initialisé. Par défaut `loadcontext` vaut
    `true`.

Exemple :

    [xml]
    <php method="Product::checkEverythingIsFine()" loadcontext="true" />

##### Process `<php-code />` {#core-ref:a7ad01e2-cc2f-4659-9c86-57a9cd93e20f}

Le process `<php-code />` permet d'évaluer du code PHP. Le code PHP est spécifié
dans la valeur textuelle de l'élément.

Les propriétés du proceess `<php-code />` sont :

- `loadcontext`: `true` ou `false`. Permet de spécifier si un contexte
    d'exécution Dynacase doit être initialisé. Par défaut `loadcontext` vaut
    `true`.

Exemple :

   [xml]
   <php-code loadcontext="true"><![CDATA[
   if (Product::migrateProducts() == '') { {
     return true;
   }
   return false;
   ]]></php-assert-code-return-true>

#### Élément `<check />` {#core-ref:cb8fa931-0ccd-4930-8ead-37c3deba3fad}

Les éléments contenus dans l'élément `<check />` sont les mêmes que ceux de
l'élément [`<condition />`][condition].

Voir sous-éléments de [`<condition />`][condition].

## Validation du fichier de migration {#core-ref:ee714280-8a4b-4fca-a50e-44c6cb220400}

Le format du fichier de migration peut être validé à l'aide de la XSD
[`migration_1.0.xsd`][migration_xsd].

    $ xmllint --schema /path/to/migration_1.0.xsd --noout FOO_postMigration.xml
    FOO_postMigration.xml validates

## Exécution du fichier de migration {#core-ref:96dcc31e-07a3-47ec-9288-e37879147912}

L'exécution du fichier de migration est à définir dans le fichier `info.xml`
par le lancement du programme `programs/migration` dans la section `<post-
upgrade/>`.

Exemple :

    <module name="foo" version="1.2.3">
    
        […]
    
        <post-upgrade>

            <!-- Exécution de la pre-migration -->
            <process command="programs/migration --file=FOO/FOO_preMigration.xml --verbose" />
    
            <process command="programs/app_post FOO U" />
            <process command="programs/record_application CORE U" />
    
            <!-- Exécution de la post-migration -->
            <process command="programs/migration --file=FOO/FOO_postMigration.xml --verbose" />
    
            <process command="programs/update_catalog" />
        </post-upgrade>
    
        […]
    
    </module>

### programs/migration {#core-ref:ea9ff0d4-63d1-49e6-9c76-8d9700057dd0}

Les arguments de `programs/migration` sont:

- `--file`: Le fichier de migration à exécuter.

<!-- links -->
[migration_xsd]: https://raw.githubusercontent.com/Anakeen/dynacase-core/3.3-integration/App/Core/migration_1.0.xsd
[migr_scripts]: #core-ref:fd259229-b279-469a-8e3c-d737fabcf9d5
[wsh_ref]: #core-ref:65862835-f70c-4cd5-bc3b-88f61d49c87a
[wsh_cli]: #core-ref:1566c46d-a53d-44cf-8c3f-0d0e21c0b117
[condition]: #core-ref:85f4eb52-6caa-44a7-8647-64bcd471c6e9
[process]: #core-ref:efb26022-63cd-4dfd-a131-18a8ab465618
[check]: #core-ref:cb8fa931-0ccd-4930-8ead-37c3deba3fad
