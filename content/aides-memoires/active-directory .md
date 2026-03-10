---
title: "Active Directory"
summary: "Mémo sur les points importants sur les active directory"
date: "2026-02-04"
categories: ["mémo"]
tags: ["active directory", "gouvernence"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## 1. Introduction à l'Active Directory

L'**Active Directory (AD)** est un service d'annuaire développé par Microsoft pour les réseaux Windows Server. Il sert de base de données centralisée contenant la liste des usagers, groupes, ordinateurs et autres ressources membres d'un domaine.

> **Rôle principal :** Authentifier et autoriser tous les utilisateurs et ordinateurs dans un réseau de type domaine Windows, en attribuant et en appliquant des politiques de sécurité.

### Concepts Fondamentaux

Un service d'annuaire est un service réseau qui identifie toutes les ressources d'un réseau et les rend accessibles aux utilisateurs et aux applications. Ces services sont importants car ils permettent de nommer, de décrire, de repérer, de gérer et de sécuriser ces ressources de manière cohérente.

* **Annuaire :** Service réseau qui identifie toutes les ressources du réseau et les rend accessibles.
* **Domaine :** Unité centrale de la structure logique d'AD. C'est une collection d'entités de sécurité comme des comptes d'utilisateurs et d'ordinateurs, imprimantes et dossiers partagés.
* **Contrôleur de Domaine (DC) :** Serveur qui répond aux demandes d'authentification de sécurité au sein d'un domaine.
* **Catalogue Global :** Un contrôleur de domaine qui enregistre une copie complète de tous les objets de l’annuaire pour son domaine hôte et une copie partielle en lecture seule de tous les objets pour tous les autres domaines de la forêt. Il est interrogé lors de l’authentification pour récupérer la liste des groupes « universels » dont l’utilisateur est membre.

---

## 2. Structure Logique de l'AD

L'organisation de l'AD repose sur une hiérarchie permettant une gestion granulaire :

| Élément | Description |
| :--- | :--- |
| **Forêt** | Le conteneur de plus haut niveau regroupant un ou plusieurs arbres de domaines. |
| **Arbre** | Hiérarchie de domaines partageant un espace de noms contigu. |
| **Domaine** | Frontière administrative et de sécurité. |
| **Unité d'Organisation (OU)** | Conteneur utilisé pour organiser les objets (utilisateurs, groupes, ordinateurs) au sein d'un domaine et y appliquer des GPO. |

---

## 3. Rôles et Services AD

Windows Server propose plusieurs rôles spécifiques pour étendre les capacités de l'annuaire :

* **AD DS (Active Directory Domain Services) :** Gestion des domaines, des utilisateurs, des groupes, et des stratégies de sécurité au sein d’un réseau.
* **AD FS (Active Directory Federation Services) :** Fournit une authentification unique (SSO) et fédère les identités pour un accès sécurisé aux applications.
* **AD CS (Active Directory Certificate Services) :** Gère les certificats numériques pour l'authentification, le chiffrement, et les signatures numériques.

---

## 4. Gestion des Objets et Sécurité

### Utilisateurs et Groupes

* **SID (Security Identifier) :** Identifiant unique pour chaque utilisateur ou groupe.
* **Types de Groupes :**
  * *Sécurité :* Permet d’attribuer des permissions.
  * *Distribution :* Utilisé pour les applications de courriels.
* **Méthode AGDLP :** Recommandation de cybersécurité pour la gestion des permissions (Accounts -> Global Groups -> Domain Local Groups -> Permissions).

### Stratégies de Groupe (GPO)

Les **GPO (Group Policy Objects)** permettent de centraliser la configuration des postes et des utilisateurs.

* **Objectifs :** Simplifier la gestion des postes pour le service informatique, activer ou restreindre certaines fonctionnalités, distribuer des applications, et centraliser le contrôle et la gestion de la sécurité des ressources informatiques.
* **Niveaux d'application :** Les stratégies de groupe peuvent être appliquées à différents niveaux : local, site, domaine, et unité d'organisation.
* **Priorité :** L'option "Refuser" est toujours prioritaire. L'héritage peut être bloqué ou forcé ("Appliqué").

---

## 5. Permissions et Partages

La gestion des accès repose sur deux types de permissions :

1. **Permissions de Partage :** S'appliquent lors de l'accès via le réseau.
2. **Permissions NTFS :** S'appliquent localement et via le réseau, offrant des options plus fines (Lecture, Écriture, Afficher le contenu du dossier, Lecture et exécution, Modifier, Contrôle total).

> **Règle d'or :** La permission effective est la somme des autorisations, mais l'option **REFUSER** est prioritaire et l'emporte toujours sur les autres autorisations. Il est recommandé d'utiliser les groupes d'utilisateurs pour attribuer collectivement des autorisations et d'éviter d'attribuer des autorisations directement aux utilisateurs sur un dossier.

---

## 6. Intégration et Évolution

* **LDAP :** L'Active Directory est une implémentation de LDAP (Lightweight Directory Access Protocol) avec des ajouts spécifiques à Microsoft. LDAP est un protocole d'accès aux informations qui permet d'échanger des renseignements entre les annuaires compatibles.
* **Azure AD (Entra ID) :** Version cloud de l'identité Microsoft, offrant des fonctionnalités de gestion d'identité et d'accès pour les applications cloud et hybrides. Il existe des différences entre l'AD local et Azure AD.
* **Linux :** Il est possible de joindre des systèmes Linux à un domaine Active Directory pour centraliser l'authentification, notamment dans des environnements mixtes Windows et Linux. Des alternatives à l'AD pour les infrastructures Linux incluent OpenLDAP et FreeIPA.

---
