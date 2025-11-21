---
title: "Mise en place d'UFW"
summary: "Tutoriel pour mettre en place Uncomplicated FireWall."
date: "2025-10-20"
categories: ["tuto"]
tags: ["linux", "shell", "firewall", "ufw"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Installation UFW

Pour unstaller le par feu Uncomplicate FireWall, il suffit d'éxecuter les commandes suivantes :

```sh
sudo apt update        # Met à jour le gestinnaire de packet
sudo apt install ufw   # Installe ufw
sudo ufw enable        # Ajoute ufw aux démarrages automatiques
sudo ufw status        # Affiche le statut de ufw
```

---

## Configuration

Par défaut, quand le par feu est activé, tous les ports sont fermés.  
Il est cependant possible d'ouvrir des ports pour autoriser les requêtes à passer par ce port.

```sh
sudo ufw allow [ARG]
```

`[ARG]` est à remplacer par le port à ouvrir sur le par feu.

Si vous voulez fermer un port ouvert, entrez la commande suivante en spécifant le port à fermer :

```sh
sudo ufw delete allow [ARG]
```

Pour afficher le status et la configuration du par feu, vous pouvez utiliser la commande suivante

```sh
sudo ufw status verbose
```

L'ajout du mot `verbose` permet d'avoir plus de détails sur la configuration.

Une configuration pour un par feu ayant les ports HTTP et HTTPS ouverts pour les adresses IPv4 et IPv6 ressemble à ça

```txt
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
80/tcp                     ALLOW IN    Anywhere
443/tcp                    ALLOW IN    Anywhere
80/tcp (v6)                ALLOW IN    Anywhere (v6)
443/tcp (v6)               ALLOW IN    Anywhere (v6)
```

---

## Exemples de port courrants

Voici une liste non exaustive de ports courant avec le protocol utilisés :

- 20, 21 (TCP): FTP
- 22 (TCP) : SSH
- 25 (TCP) : SMTP
- 80 (TCP): HTTP
- 389 (TCP/UDP) : LDAP
- 443 (TCP) : HTTPS
- 465, 587 (TCP): SMTP sécurisé
- 636 (TCP) : LDAPS
- 1433 (TCP) : Microsoft SQL Server
- 1521 (TCP) : Oracle Database
- 3306 (TCP) : MySQL/MariaDB
- 3389 (TCP) : RDP
- 5432 (TCP) : PostgreSQL
- 27017 (TCP) : MongoDB

---
