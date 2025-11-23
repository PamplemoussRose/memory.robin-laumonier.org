---
title: "Mise en place d'un proxy inverse"
summary: "Tutoriel pour mettre en place un proxy inverse via Nginx."
date: "2025-11-23"
categories: ["tuto"]
tags: ["linux", "serveur web", "nginx", "grafana"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

## Prérequis

Avant de réaliser les étapes suivante, vous devez avoir mis en place un tableau de bord utilisant Grafana et avoir un serveur Nginx fonctionnel.

Vous pouvez mettre en place cette installation en suivant les tutoriels suivants

- [**Mise en place d'un dashboard Grafana**](https://memory.robin-laumonier.org/aides-memoires/dashboard-ressources/)
- [**Serveur web auto-hébergé**](https://memory.robin-laumonier.org/aides-memoires/serveur-nginx/)

---

## Situation actuelle

Dans notre situation nous avons mis en place un tableau de bord utilisant Grafan qui récupère des métriques sur notre machine et nous permet de les visualiser.

Ce tableau de bord est pour l'instant uniquement accéssible sur la machine via le localhost.

Notre but est de rendre ce tableau de bord accéssible via une URL pour pouvoir visualier les données sans être directement sur la machine.

Pour ce faire nous allons configurer un proxy inverse (ou *reverse proxy* en anglais) pour rediriger les requètes demandant un certain site vers le localhost de la machine.

---

## Mise en place du proxy inverse (revers proxy)

Pour mettre en place nous devons modifier le fichier de configuration Grafana et créer un nouveau site sur le serveur Nginx.

### Grafana

Du coté de Grafana, il y a deux lignes à modifier dans le fichier `/etc/grafana/grafana.ini` :

```ini
[server]

# Indique à Grafana l'URL complète par laquelle il sera accessible
root_url = %(protocol)s://%(domain)s/grafana/

# Indique à Grafana le sous-chemin
serve_from_sub_path = true
```

Ces changements permettent d'avoir accès à Grafan via une URL autre que le localhost et précisent que l'interface peut être accéssible via un sous chemin.

### Nginx

Pour Nginx, nous devons créer un nouveau site.

A la différence des sites classiques, celui-la n'a pas besoin d'avoir de fichier à afficher. Il existe uniquement pour faire proxy.

Nous avons donc justa à créer le fichier de configuration et à faire le lien symbolique pour activer le site.

Fichier de configuration `/etc/nginx/sites-available/dashboard.domaine.org` :

```txt
server {
    # Port d'écoute
    listen [::]:80;
    listen 80;
    # Nom de domaine que ce bloc de serveur doit gérer
    server_name dashboard.domaine.org;

    # Comportement pour les requètes 'dashboard.domaine.org/grafana/'
    location /grafana/ {
        # Adresse de redirection
        proxy_pass http://localhost:3000/grafana/;
        # Transmission des headers
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Création du lien symbolique :

```sh
sudo ln -s /etc/nginx/sites-available/dashboard.domaine.org /etc/nginx/sites-enabled/
```

Vous pouvez maintenant créer l'enregistrement DNS sur votre domaine pour rediriger les requètes `dashboard.domaine.org` vers l'adresse IP de votre routeur internet.

---

## Mise en place du HTTPS

La mise en place de la connexion HTTPS n'est pas differente des autres sites. Utilisez la commande suivante génèrera un certificat pour le site `dashboard.domaine.org` .

```sh
sudo certbot --nginx -d dashboard.domaine.org
```

Une fois le certificat installé, votre fichier de configuration pour le site `dashboard.domaine.org` devrait ressembler à ça :

```txt
server {
    # Nom de domaine que ce bloc de serveur doit gérer
    server_name dashboard.domaine.org;

    location /grafana/ {
        proxy_pass http://localhost:3000/grafana/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    listen [::]:443 ssl; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/dashboard.domaine.org/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/dashboard.domaine.org/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}
server {
    if ($host = dashboard.domaine.org) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    server_name dashboard.domaine.org;
    listen [::]:80;
    listen 80;
    return 404; # managed by Certbot
}
```

---
