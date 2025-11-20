---
title: "Connexion RDP"
summary: "Tutoriel pour mettre en place une connexion RDP vers un appareil."
date: "2025-10-19"
categories: ["tuto"]
tags: ["windows", "rdp"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Vers une machine Windows Pro

### Windows Pro - Installation

Aucune installation nécessaire. Tout le nécessaire est déjà installé.

### Windows Pro - Connexion

1. Ouvrez l'application *Connexion bureau à distance* (cherchez 'rdp' dans la barre de recherche Windows)
2. Entrez l'adresse IP de la machine à laquelle vous voulez vous connecter
3. Entrez l'identifiant et le mot de passe pour la session à ouvrir

---

## Vers une machine Windows Famille

### Windows Famille - Installation

La version familiale ne supportant pas la connexion rdp, nous allons utiliser un logiciel tiers, *TeamViewer*.

1. Téléchargez et installez l'application *TeamViewer* (**<https://www.teamviewer.com/fr/download/windows/>**) sur vos deux appareils
2. Créez un compte si ce n'est pas déjà fait et connectez-vous une fois l'application lancée
3. Dans l'onglet *Téléassistance*, cochez la case *Autoriser Grant Access sur cet appareil*

### Windows Famille - Connexion

1. Démarrez *TeamViewer*
2. Allez dans l'onglet Appareils
3. Sélectionnez et connectez-vous à l'appareil que vous désirez

---

## Vers une machine Linux

### Linux - Installation

Installez et lancez le logiciel *xrdp* sur votre machine Linux.

```sh
sudo apt update             # Met à jour le gestionnaire apt
sudo apt upgrade -y         # Met à jour les paquets installés
sudo apt install xrdp -y    # Installe xrdp
sudo systemctl enable xrdp  # Ajoute xrdp aux démarrages automatiques
sudo systemctl start xrdp   # Lance xrdp
```

Vérifiez que le port 3389 est ouvert pour les adresses IPv4 comme IPv6.

```sh
sudo ufw allow 3389/tcp     # Exemple avec le firewall UFW
```

Vérifiez l'état de *xrdp* avec la commande.

```sh
systemctl status xrdp
```

### Linux - Connexion

1. Ouvrez l'application *Connexion bureau à distance* (cherchez 'rdp' dans la barre de recherche Windows)
2. Entrez l'adresse IP de la machine à laquelle vous voulez vous connecter
3. Entrez l'identifiant et le mot de passe pour la session à ouvrir dans l'interface *xrdp*

---
