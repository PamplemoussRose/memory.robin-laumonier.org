---
title: "Serveur web auto-hébergé"
summary: "Tutoriel pour mettre en place un serveur web auto-hébergé avec Nginx sur une machine linux."
date: "2025-11-16"
categories: ["tuto"]
tags: ["linux", "shell", "nginx", "serveur web", "site web"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Installation de Nginx

Pour installer Nginx, il suffit d'exécuter les commandes suivantes :

```sh
sudo apt update                 # Met à jour le gestionnaire apt
sudo apt install nginx curl -y  # Installe nginx
sudo systemctl enable nginx     # Ajoute nginx au démarrage automatique
sudo systemctl start nginx      # Lance nginx
```

---

## Mise en place d'un site

Une fois que Nginx est installé, nous pouvons commencer la configuration du site.

{{< alert "circle-info">}}
Dans les commandes suivantes, le site est nommé *mon_site_web*. Toutes les commandes sont donc à adapter selon le nom de votre site.
{{< /alert >}}

Tout d'abord, nous devons créer le dossier où seront stockés les fichiers du site puis attribuer les bonnes permissions.  
Vous pouvez mettre une page temporaire dans ce dossier ou votre site s'il existe déjà.

```sh
# Crée le répertoire racine du site
sudo mkdir -p /var/www/mon_site_web

# Définit les permissions
sudo chown -R www-data:www-data /var/www/mon_site_web
sudo chmod -R 755 /var/www/mon_site_web
```

L'utilisateur `www-data` est celui utilisé par Nginx. C'est la raison pour laquelle il est mis comme propriétaire du dossier.

Nginx gère la disponibilité des sites via deux dossiers :

- `sites-available` : Contient les fichiers de configuration des sites sur le serveur
- `sites-enabled` : Contient des liens symboliques vers les fichiers de configurations. Seuls les sites ayant un lien symboliques sont accessible via le serveur.

Nous devons donc commencer par créer le fichier de configuration du site.

```sh
sudo nano /etc/nginx/sites-available/mon_site_web
```

Ce fichier suit le format suivant :

```txt
server {
    # Port d'écoute
    listen 80;        # IPv4
    listen [::]:80;   # IPv6

    # Nom du site
    server_name mon_site_web;

    # Chemin racine du site
    root /var/www/mon_site_web;
    # Ficher à chercher à la racine
    index index.html;

    # Comportement lors de la réception d'une requête
    location / {
        try_files $uri $uri/ =404;
    }
}
```

Une fois que le site est mit comme étant disponible, nous devons l'activer en créant un lien symbolique vers le fichier de configuration dans le dossier des sites activés.

```sh
# Créer un lien symbolique pour activer la configuration
sudo ln -s /etc/nginx/sites-available/mon_site_web /etc/nginx/sites-enabled/
```

Nginx propose une commande pour vérifier qu'il n'y a pas d'erreurs de syntaxe dans les fichiers de configuration.  
Si le test réussit, vous pouvez redémarrer Nginx pour appliquer les changements.

```sh
# Tester la configuration
sudo nginx -t

# Redémarrer Nginx
sudo systemctl restart nginx
```

Votre site est maintenant accessible en HTTP via l'adresse IP de votre routeur ou via une URL si vous avez paramétré un DNS.

---

## Activation de la connexion HTTPS

Votre site est accessible mais uniquement en HTTP. Il est recommandé de sécuriser votre serveur en autorisant uniquement les connexions via HTTPS.

Pour ce faire, nous allons utiliser certbot qui permet la génération et le renouvellement de certificats.

### Mode SSL/TLS

Avant de commencer avec certbot, il est vivement recommandé de passer le chiffrement SSL/TLS en mode *Full (Strict)* sur Cloudflare.

Pour cela, allez dans votre compte Cloudflare, partie SSL/TLS, onglet *Overview, puis cliquez sur *Configure* pour changer de mode. Ensuite, choisissez *Custom SSL/TLS* puis sélectionnez *Full (Strict)*. Vous pouvez maintenant cliquez sur *Save* et retourner sur votre terminal.

### Installation de certbot

Pour installer certbot, utilisez la commande suivante :

```sh
# Installer Certbot et le plugin Nginx
sudo apt install certbot python3-certbot-nginx -y
```

### Génération du certificat

Maintenant que certbot est installé, nous pouvons lancer la génération du certificat avec la commande suivante :

```sh
sudo certbot --nginx -d cv.robin-laumonier.org
```

Une fois la commande lancée, suivez les instructions à l'écran. Une des question vous demandera de choisir entre retirer la connexion HTTP ou la rediriger vers la connexion HTTPS. Il est recommandé de rediriger les connexions HTTP vers celles HTTPS.

Une fois le certificat généré, certbot fait des changements dans le fichier de configuration de votre site. Après modification, il doit avoir le format suivant :

```txt
server {
    # Nom du site
    server_name mon_site_web;

    # Chemin racine du site
    root /var/www/mon_site_web;
    # Ficher à rechercher à la racine
    index index.html;

    # Comportement lors de la réception d'une requête
    location / {
        try_files $uri $uri/ =404;
    }

    listen [::]:443 ssl; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/mon_site_web/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/mon_site_web/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}
server {
    if ($host = mon_site_web) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    listen 80;
    listen [::]:80;
    server_name mon_site_web;
    return 404; # managed by Certbot
}
```

### Renouvellement de certificat

Les certificats sont valables 90 jours. Cependant, certbot a automatiquement configuré une tâche cron ou un timer `systemd` pour le renouvellement.

Vous pouvez tester le processus avec la commande suivante. Si elle réussit, le renouvellement automatique fonctionnera correctement.

```sh
sudo certbot renew --dry-run
```

---
