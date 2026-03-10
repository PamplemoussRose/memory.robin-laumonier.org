---
title: "Synchronisation de fichiers"
summary: "Mis en place d'une synchronisation de fichiers entre deux appareils via Syncthing"
date: "2026-02-11"
categories: ["tuto"]
tags: ["données", "sauvegarde", "syncthing", "redondance"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## 1. Installation de Syncthing sur les ordinateurs

### Sous Windows

- Téléchargez et installez Syncthing depuis la liste [syncthing.net/downloads](https://syncthing.net/downloads/)
- Une fois installé, Syncthing devrait démarrer et être accessible via votre navigateur web (à l'adresse [http://localhost:8384](http://localhost:8384) avec la configuration par défaut).

### Sous Linux

Installez Syncthing via la commande suivante :

```bash
sudo apt install syncthing
```

Ensuite, activez et démarrez le service avec les commandes suivantes :

```sh
sudo systemctl enable syncthing@<nom_utilisateur>.service
sudo systemctl start syncthing@<nom_utilisateur>.service
```

Vous pouvez enfin vérifier le status du service avec la commande suivante :

```sh
systemctl status syncthing@<nom_utilisateur>.service
```

## 3. Connexion des Appareils Syncthing

Une fois que Syncthing est installé sur vos appareils, vous devez les ajouter entre eux.

Pour ce faire, allez dans le menu `Action`, puis cliquez sur `Afficher mon ID`. Ensuite, sur votre second appareil, cliquez sur `Ajouter un appareil` dans la partie `Autres appareils` puis entrez l'ID du premier appareil pour l'ajouter. Faites la même chose dans l'autre sens pour ajouter le second appareil au premier.

Une fois un appareil ajouté, il doit apparaître avec le statut `Connecté` dans l'interface d'administration.

## 4. Configuration du Dossier Partagé Syncthing

Pour créer un partage entre deux appareils, il suffit d'entrer le chemin du dossier à partager, les appareils inclus ainsi que la relation de partage.  
Chaque appareil dans le partage peut avoir une relation différente tel que `Envoi & réception`, `Envoi seulement` ou encore `Réception seulement`.

Une fois le partage créé, une invitation sera envoyée à tous les membres du partage. Ils devront alors choisir le dossier local lié au partage ainsi que le type de relation. Syncthing analysera automatiquement les différences entre les dossiers et commencera la synchronisation selon les relations configurées.

---
