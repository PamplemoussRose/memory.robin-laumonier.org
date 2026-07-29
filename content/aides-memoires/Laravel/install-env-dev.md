---
title: "Installation et accès au projet"
summary: "Partie du tuto Laravel pour mettre en place l'environnement de developpement et acceder au site"
date: "2026-07-31"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Instalation de l'environement de développement

### Développement

Pour pouvoir développer en utilisant le framework Laravel, vous devez avoir un environement comprenant :

- `php`
- `composer`
- `laravel`
- `expose`
- `node`
- `npm`
- `nvm`

Vous pouvez soit les installer manuellement, soit utiliser Laravel Herd ([**https://herd.laravel.com/**](https://herd.laravel.com/)) qui est un utilitaire qui va se charger de tout installer pour vous.  
La version gratuite est suffisante pour créer un site.

### Base de données

Par défaut, Herd utilise SQLite comme base de données. Si vous souhaitez utiliser un autre driver, veillez à l'installer pour la suite.

---

## Creation d'une application Laravel

Pour la suite du tutoriel, nous allons assumer que :

- L'installation a été réalisée via Herd
- Le driver de base de données utilisé est MySQL
- Aucun *starter kit* n'est utilsé
- Les pages seront des templates *Blade*

### Creation de l'application

Pour créer une application Laravel, rendez-vous dans le dossier Herd qui est par défaut `C:\Users\user\Herd` puis éxecutez la commande suivante :

```sh
laravel new nom_app
```

Ensuite, suivez la configuration dans le terminal pour finaliser la création.  
Vous pouvez également utiliser un des *Starter Kits* disponibles nativement avec Laravel.

### Modification de la base de donées

Pour changer la base de données utilisée par l'applciation, vous devez modifier le fichier `.env` qui se trouve dans le dossier `~\Herd\nom_app`.

Les variables à changer sont :

- `DB_CONNECTION`, le driver de base de données utiliser (ex : mysql, sqlite...)
- `DB_HOST`, l'adresse pour contacter la base de données
- `DB_PORT`, le port utilisé par la base de données
- `DB_DATABASE`, le nom de la base de données
- `DB_USERNAME`, le nom de l'utilisateur que va utiliser l'application
- `DB_PASSWORD`, le mort de passe de l'utilisateur que va utiliser l'application

### Accès à l'application

Une fois la configuration terminée, vous pouvez accéder à l'application via l'URL `http://nom_app.test` (uniquement si l'instalation a été réalisée avec Herd).

---
