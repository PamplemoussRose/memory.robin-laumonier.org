---
title: "Connexion SSH avec clés RSA"
summary: "Tutoriel pour mettre en place une connexion SSH sécurisée par clés RSA vers un appareil."
date: "2025-10-18"
categories: ["tuto"]
tags: ["linux", "shell", "ssh", "rsa"]
layout: "page"
draft: "false" # Set to true if this page is not to be show
---

---

## Connexion SSH

### Installation d'OpenSSH

Pour commancer il faut installer le serveur ssh sur la machine cilbe.  
Nous allons utiliser *OpenSSH* pour ce tutoriel.

```sh
sudo apt update                  # Met à jours le gestionaire apt
sudo apt install openssh-server  # Installe OpenSSH
sudo systemctl start ssh         # Démarre OpenSSH
sudo systemctl enable ssh        # Ajoute xrdp aux démarages auto
sudo systemctl status ssh        # Affiche le statut de Open SSH
```

Ensuite vérifiez que le port utilisé par les connexions SSH est bien ouvert.

```sh
sudo ufw allow ssh  # Exemple avec le fire wall UFW
```

### Connexion à la machine cible

Pour vous connecter à la machine cible, vous avez besoin de son adresse ip ainsi que du nom de l'utilisateur auquel vous voulez vous connecter.

Pour avoir l'adresse ip, entrez la commande suivante dans votre machine cible.

```sh
ip -br a
```

Une fois que vous avez l'adresse, vous pouvez vous connecter en utilsant la commande suivante sur votre machine source.

```sh
ssh ssh nom_utilisateur@adresse_ip
```

- `nom_utilisateur` correspond à la session avec laquelle vosu allez être connecté
- `adresse_ip` est l'adresse de la machine cible obtenue avec la commande précédente

Lors de la première connexion, un message vous demandera de vérifier l'authenticité de l'hôte en acceptant son empreinte de clé ("fingerprint").

Pour compléter la connexion, vous devez entrer le mot de passe de l'utilisateur avec lequel vouv vous connectez et vous aurrez accès à un shell relié à votre machine cible.

---

## Mise en place des clés RSA

En suivant les étpes précedentes, vosu avez mis en place une connexion ssh entre deux machines. L'étape suivant est de sécuriser la connexion.

Pour ce faire nosu allons mettre en place une connexion par clé RSA. Lors de la connexion SSH, le serveur ne va plus vosu demander le mot de passe de l'utilisateur utilisé pour la connexion mais va plutôt regarder si vosu possédez la clé 

### Générer les clés

### Copie de la clé publique sur la machine cible

### Test de la connexion par clé

---
