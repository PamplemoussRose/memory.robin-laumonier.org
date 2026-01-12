---
title: "Installation de Moonlight et Sunshine"
summary: "Mise en place de Moonlight et Sunshine pour pouvoir jouer sur un ordinateur distant."
date: "2025-12-01"
categories: ["tuto"]
tags: ["jeu", "moonlight", "sunshine"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Installation du serveur Sunshine (sur le PC de Jeu distant)

Sunshine est le logiciel serveur qui s'exécute sur votre PC de jeu distant. Il est compatible avec les cartes graphiques NVIDIA, AMD et Intel.

### Prérequis

* Un PC de jeu (Windows, Linux ou macOS) avec une carte graphique dédiée (NVIDIA, AMD ou Intel).
* Une connexion Internet stable et rapide (fibre optique recommandée pour le jeu à distance via Internet).

### Procédure d'installation (Windows - Recommandé)

1. **Téléchargement :** Rendez-vous sur la page des [dernières versions de Sunshine](https://github.com/LizardByte/Sunshine/releases) et téléchargez le fichier `Sunshine-Windows-AMD64-installer.exe`.
2. **Installation :** Exécutez le fichier d'installation. L'installeur est le moyen le plus simple de configurer Sunshine en tant que service Windows.
3. **Configuration initiale :**
    * Une fois l'installation terminée, Sunshine devrait démarrer automatiquement et ouvrir une page de configuration dans votre navigateur web (généralement `https://localhost:47990`).
    * Le navigateur vous avertira que la connexion n'est pas sécurisée (certificat auto-signé). Vous devez accepter le risque et continuer.
    * **Création de l'utilisateur :** Vous serez invité à créer un nom d'utilisateur et un mot de passe pour accéder à l'interface web de configuration. **Ceci n'est pas le code de jumelage.**
    * **Vérification :** Dans l'interface web, naviguez vers l'onglet **Configuration** pour vérifier que tout est correctement configuré.

---

## Installation du client Moonlight (sur l'Appareil client)

Moonlight est le client qui s'exécute sur l'appareil que vous utilisez pour jouer (ordinateur portable, smartphone, tablette, etc.).

### Procédure d'installation

Moonlight est disponible pour une grande variété de plateformes :

1. **Windows, macOS, Linux :** Téléchargez la version appropriée sur la page [Client Downloads](https://moonlight-stream.org/) ou directement sur le [GitHub de Moonlight-qt](https://github.com/moonlight-stream/moonlight-qt/releases).
2. **Android :** Téléchargez l'application **Moonlight Game Streaming** sur le [Google Play Store](https://play.google.com/store/apps/details?id=com.limelight).
3. **iOS/iPadOS/tvOS :** Téléchargez l'application **Moonlight Game Streaming** sur l'[App Store](https://apps.apple.com/us/app/moonlight-game-streaming/id1000551566).

---

## Jumelage et Streaming

1. **Démarrage du Client :** Lancez l'application Moonlight sur votre appareil client.
2. **Détection du PC :** Si vous êtes sur le même réseau local (LAN), votre PC de jeu distant (avec Sunshine) devrait apparaître automatiquement. Si vous êtes sur Internet, vous devrez peut-être ajouter manuellement l'adresse IP publique de votre PC de jeu.
3. **Jumelage (Pairing) :**
    * Cliquez sur l'icône de votre PC dans Moonlight.
    * Moonlight affichera un **code PIN à 4 chiffres**.
    * Sur votre PC de jeu distant, l'interface web de Sunshine affichera une boîte de dialogue vous demandant de saisir ce code PIN.
    * Saisissez le code PIN dans Sunshine pour jumeler les deux appareils.
4. **Lancement du Jeu :** Une fois le jumelage réussi, Moonlight affichera la liste des jeux détectés par Sunshine. Cliquez sur un jeu pour commencer le streaming.

---
