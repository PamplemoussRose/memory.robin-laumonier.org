---
title: "Démarrage automatique d'application"
summary: "Comment configurer les applications à lancer au démarrage de l'ordinateur"
date: "2026-02-12"
categories: ["mémo"]
tags: ["windows", "automatisation"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Gestionnaire de tâches

- Appuyez sur `ctrl` + `maj` + `esc` pour ouvrir le gestionnaire de tâches
- Allez dans l'onglet `Application de démarrage`
- Sélectionnez le programme voulu et passez son statut à :
  - `Activé` si vous voulez que le programme démarre avec la machine
  - `Désactivé` si vous ne voulez pas que le programme démarre avec la machine

Si le programme que vous cherchez n'est pas dans la liste du gestionnaire de tâches, passez à la partie suivante.

---

## Dossier de démarrage

- Appuyez sur `win` + `R` pour ouvrir la boîte de dialogue Exécuter
- Tapez `shell:startup` et appuyez sur `Entrée` pour ouvrir le dossier de démarrage
- Copiez-collez le raccourci de votre application dans le dossier
- Vous pouvez alors modifier le statut de l'application dans le gestionnaire de tâches (il sera à `Activé` par défaut)

---
