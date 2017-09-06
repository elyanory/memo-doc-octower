# Guide d'utilisation Octower
    
    
## Pré-requis
    
- Octower

## Installation
    
Installer le [repository](https://github.com/octower/octower) sur **tous** les environnements (local/dev/prod), cela va créer un fichier octower.phar :
```bash
curl -sS https://getoctower.org/installer | php
```

## Configuration
    
1. Créer un fichier __octower.json__ sur votre environnement local.

Exemple pour un site en symfony2 :

```json
{
    "name": "NOM",
    "description": "DESCRIPTION",
    "type": "project",
    "shared": {
        "app/cache/": {
            "generator": "folder-empty"
        },
        "app/logs/": {
            "generator": "folder-empty"
        },
        "uploads/": {
            "generator": "folder-empty"
        },
        "app/config/parameters.yml": {
            "generator": "file-dist"
        }
    },
    "excluded": [
        ".vagrant",
        "Vagrantfile",
        "import/",
        "composer.json",
        "composer.lock",
        "composer.phar",
        ".idea/",
        ".gitignore"
    ],
    "config": {
        "vendor-dir": "vendor/"
    },
    "remotes": {},
    "scripts": {
        "post-extract": [
            {"command": "chmod +x bin/clean-appkernel.sh", "priority": 11},
            {"command": "./bin/clean-appkernel.sh", "priority": 20}
        ],
        "pre-enable": [
            {"command": "rm app/cache/* -Rf", "priority": 10},
            {"command": "php app/console assets:install --symlink --relative", "priority": 20}
        ]
    }
}
```

2. Sur le serveur, initialiser la configuration :

```bash
php octower.phar server:init
```

## Déploiement
    
1. Sur le local, il faut commiter les modifications et tagguer une version puis :
```bash
php octower.phar package:generate
```

2. Envoyer l'archive générée sur le serveur de prod puis :
```bash
php octower.phar server:package:extract nom-de-ton-archive
```

3. Il suffit ensuite d'activer la bonne version du projet sur la production :
```bash
php octower.phar server:release:list # Liste toutes les versions avec leurs numéros.
php octower.phar server:release:enable numero-de-version # Active une version
```
