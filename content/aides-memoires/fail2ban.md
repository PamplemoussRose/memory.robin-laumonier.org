---
title: "Installation de Fail2Ban"
summary: "Tutoriel pour mettre en place le paquet fail2ban"
date: "2025-12-28"
categories: ["tuto"]
tags: ["fail2ban", "connexion", "log"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

## Installation

Pour installer `fail2ban`, il suffit d'exécuter les commandes suivantes :

```sh
sudo apt update               # Met à jour le gestionnaire de paquets
sudo apt install fail2ban     # Installe fail2ban
systemctl start fail2ban      # Démarre fail2ban
systemctl enable fail2ban     # Ajoute fail2ban au démarrage automatique
systemctl status fail2ban     # Affiche le statut de fail2ban
```

Si vous avez `Active: failed` avec les erreurs  
`ERROR Failed during configuration: Have not found any log file for sshd jail`  
et  
`ERROR Async configuration of server failed`  
lors de l'affichage du status de `fail2ban`, vous devez apporter les corrections suivantes :

- Créer le fichier de configuration `jail.local` :

```sh
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

- Dans la section `[DEFAULT]`, changer la valeur de la directive `backend` à `systemd`
- Dans la section `[sshd]`, ajouter une directive `enable` avec la valeur `true`
- Redémarrer `fail2ban` avec la commande suivante :

```sh
sudo systemctl restart fail2ban
```

Une fois le correctif appliqué et `fail2ban` redémarré, vous devriez avoir `Active: active (running)` lors de l'affichage du status.

---

## Configuration

Tous les changements de configurations doivent être faits dans le fichier `jail.local`.  
Il n'est pas recommandé de les faire dans le fichier `jail.conf` car celui-ci peut être écrasé lors d'une mise à jour du paquet.

La directive `ignoreip` permet de définir les IP qui ne sont pas concernées par le contrôle.

```txt
ignoreip = 127.0.0.1 124.32.5.48
```

La directive `findtime` définit le temps depuis lequel une anomalie est recherchée dans les logs.  
La durée peut être en secondes (s), en minutes (m) ou en heures (h).

```txt
findtime  = 10m
```

La directive `bantime` définit la durée de bannissement d'une adresse IP.  
La durée peut être en secondes (s), en minutes (m) ou en heures (h).


```local
bantime = 24h
```

La directive `maxretry` définit le nombre maximum de tentatives de connexion avortées avant un bannissement.

```txt
maxretry = 5
```

La directive `backend` définit le système utilisé pour récupérer les logs.  
Les options sont :

- `pyinotify` (Nécessite d'avoir `pyinotify` d'installé)
- `gamin` (Nécessite d'avoir `gamin` d'installé)
- `polling`
- `auto` (Essaiera d’utiliser les options ci-dessus dans l'ordre `pyinotify`, `gamin`, `polling`.)
- `systemd`

```txt
backend = systemd
```

Après avoir modifié le fichier de configuration, vous devez redémarrer `fail2ban` pour que les changements soient pris en compte.

```sh
sudo systemctl restart fail2ban
```

---

## Informations sur les jails (prisons)

Liste des jails :

```sh
sudo fail2ban-client status
```

Détail d'une jail :

```sh
sudo fail2ban-client status [NOM_PRISON]
```

---

## Bannissement et débannissement manuel

Bannir une IP :

```sh
fail2ban-client set [NOM_PRISON] banip [ADRESSE_IP]
```

Dé-bannir une IP :

```sh
fail2ban-client set [NOM_PRISON] unbanip [ADRESSE_IP]
```

---
