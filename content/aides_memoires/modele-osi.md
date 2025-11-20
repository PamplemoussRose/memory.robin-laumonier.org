---
title: "Modèle OSI"
summary: "Explication des couches du modèle OSI."
date: "2025-11-20"
categories: ["mémo"]
tags: ["réseau", "coommunication"]
layout: "page"
draft: "false" # Set to true if this page is not to be show
---

---

## Le modèle OSI

Le modèle OSI est un modèle conceptuel de référence qui décrit les sept couches de communication d'un réseau informatique pour permettre l'interopérabilité entre différents systèmes. Chaque couche a un rôle spécifique, allant de l'envoi des signaux physiques (couche 1) à l'interface utilisateur (couche 7). Il aide à concevoir des applications et du matériel réseau, à comprendre le flux de données et à résoudre les problèmes.  

![Le modèle OSI](modele-osi.png)

---

## Les sept couches du modèle OSI

### Couche 1 : Physique

Transporte des bits bruts sur un support. Définit câbles, signaux électriques, modulation, connecteurs, topologies, débit binaire. Aucun traitement logique des données.

### Couche 2 : Liaison de données

Fournit une transmission fiable entre deux équipements directement connectés. Encadre les bits en trames, gère adresses MAC, détection/correction d’erreurs locale, contrôle de flux, accès au média (CSMA/CD, CSMA/CA). Deux sous-couches : LLC et MAC.

### Couche 3 : Réseau

Assure l’acheminement d’un paquet d’une source à une destination via plusieurs réseaux. Fournit adressage logique (IPv4/IPv6), routage, fragmentation, calcul de routes, commutation de paquets.

### Couche 4 : Transport

Garantit le transport de bout en bout entre applications. Segmentation/reassemblage, fiabilité optionnelle, contrôle de flux, contrôle de congestion, multiplexage. Protocoles typiques : TCP, UDP.

### Couche 5 : Session

Gère l’établissement, le maintien et la terminaison des sessions entre applications. Synchronisation, points de reprise, contrôle du dialogue.

### Couche 6 : Présentation

Assure la traduction et la transformation des données. Encodage, compression, chiffrement, représentation syntaxique commune entre systèmes hétérogènes.

### Couche 7 : Application

Interface directe avec les services applicatifs. Définit protocoles et formats utilisés par les applications réseau : HTTP, FTP, SMTP, DNS, etc.

---

## Moyen mémo-technique

"Petit Lapin Rose Trouvé à la SPA"

- Petit : Physique
- Lapin : Liaison
- Rose : Réseau
- Trouvé : Transport
- SPA : Séssion, Présentation, Application

---
