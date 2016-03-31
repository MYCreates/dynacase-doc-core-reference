# Ajouter une nouvelle langue {#core-ref:2b79d76a-fe0d-446e-bee4-c73e36f64a41}

Pour disposer d'une nouvelle langue, il faut

-   Ajouter un répertoire `locale/<lang>`,
-   ajouter le fichier `locale/<lang>/lang.php` de définition de langue,
-   ajouter un répertoire `locale/<lang>/LC_MESSAGES`,
-   ajouter le fichier
    [`locale/<lang>/LC_MESSAGES/header.mo` (définissant notamment les formes plurielles)][core-ref:locale_header]
-   vous assurer que la locale existe sur le serveur

## Exemple : ajout de l'espagnol {#core-ref:b87c19ac-1fdc-4724-9a58-751c42abac62}

### Fichier `locale/es/lang.php` {#core-ref:d1435a15-deb7-4376-8978-6b23fc9ae3b0}

    [php]
    $lang["es_ES"] = array(
        "label"             => "Espagnol",          // le label non traduit
        "localeLabel"       => _("Spanish"),        // le label traduit
        "flag"              => "es_ES.png",         // le nom du fichier de drapeau de la locale
        "locale"            => "es",                // le code ISO 639-1 de la locale
        "dateFormat"        => "%d/%m/%Y",          // le format de date utilisable par strftime
        "dateTimeFormat"    => "%d/%m/%Y %H:%M",    // le format de datetime utilisable par strftime
        "timeFormat"        => "%H:%M:%S",          // le format d'heure utilisable par strftime
        "culture"           => "es-ES"              // le code RFC 3066 de la locale
    );
    /*
     ** Include local/override config
     ** -----------------------------
    */
    $local_lang = dirname(__FILE__) . DIRECTORY_SEPARATOR . 'local-lang.php';
    if (file_exists($local_lang)) {
        include ($local_lang);
    }

### Fichier `locale/es/LC_MESSAGES/header.mo` {#core-ref:bc9f6ff1-c901-4a97-9897-8661685ca605}

Il résulte de la compilation par `msgfmt` du fichier po suivant :

    [gettext]
    msgid ""
    msgstr ""
    "Project-Id-Version: Dynacase\n"
    "Report-Msgid-Bugs-To: \n"
    "PO-Revision-Date: 2013-10-28 08:59+0100\n"
    "Language: es\n"
    "Language-Team: Spanish team\n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"
    "Plural-Forms: nplurals=2; plural=(n > 1);\n"

<!-- link -->
[core-ref:locale_header]:   #core-ref:a59b5c3f-502d-4679-8197-7654b4e2a5bb