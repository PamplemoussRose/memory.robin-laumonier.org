---
title: "Framework NIST CSF 2.0"
summary: "Guide sur le Framework NIST CSF 2.0 avec explication de chaque fonction et exemples d'actions pour chacune d'elles."
date: "2026-03-09"
categories: ["mémo"]
tags: ["cyber-secu", "norme", "framework", "NIST"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

Le **NIST Cybersecurity Framework (CSF) 2.0**, publié en février 2024, est une évolution majeure du cadre de référence mondial pour la gestion des risques de cybersécurité. Le NIST CSF est un cadre flexible axé sur les **résultats** (outcomes) plutôt que sur des contrôles rigides.

---

## Structure du NIST CSF 2.0

Le cœur du framework est organisé autour de **6 Fonctions** clés (contre 5 dans la version 1.1). Ces fonctions représentent les piliers d'un programme de cybersécurité complet et holistique.

| Fonction | Description Simplifiée |
| :--- | :--- |
| **GOVERN (Gouverner)** | Établir et surveiller la stratégie, l'éthique et la gestion des risques. |
| **IDENTIFY (Identifier)** | Comprendre les actifs, les données et les risques de l'organisation. |
| **PROTECT (Protéger)** | Mettre en place des garde-fous pour limiter l'impact d'une menace. |
| **DETECT (Détecter)** | Identifier l'occurrence d'un événement de cybersécurité. |
| **RESPOND (Répondre)** | Agir suite à la détection d'un incident de cybersécurité. |
| **RECOVER (Restaurer)** | Rétablir les services et capacités impactés par un incident. |

---

## Détail des Fonctions et Actions Concrètes

### 1. GOVERN (Gouverner) - *La Nouveauté 2.0*

**Importance :** Cette fonction souligne que la cybersécurité est un enjeu de direction et non seulement technique. Elle couvre la stratégie, la gestion des risques tiers et les politiques.

**Exemples d'actions :**

* **Contexte Organisationnel :** Identifier et documenter les exigences légales et réglementaires (ex: RGPD, NIS 2).
* **Stratégie de Gestion des Risques :** Établir un "appétit au risque" clair validé par le conseil d'administration.
* **Gestion des Risques Tiers :** Évaluer la posture de sécurité des fournisseurs critiques avant signature de contrat.
* **Rôles et Responsabilités :** Créer une matrice RACI pour les décisions de cybersécurité.

### 2. IDENTIFY (Identifier)

**Importance :** On ne peut pas protéger ce que l'on ne connaît pas. Cette fonction se concentre sur l'inventaire et la compréhension de l'écosystème.

**Exemples d'actions :**

* **Gestion des Actifs :** Maintenir un inventaire automatisé de tous les équipements (PC, serveurs, IoT) et logiciels.
* **Évaluation des Risques :** Réaliser des analyses de risques régulières sur les processus métiers critiques.
* **Amélioration :** Identifier les lacunes du programme de sécurité via des auto-évaluations ou des audits.

### 3. PROTECT (Protéger)

**Importance :** Prévenir ou limiter les dommages. C'est ici que se trouvent la plupart des mesures techniques de défense.

**Exemples d'actions :**

* **Gestion des Identités et Accès :** Imposer l'authentification multi-facteurs (MFA) pour tous les accès distants et privilégiés.
* **Sensibilisation :** Mener des campagnes de simulation de phishing mensuelles pour former les employés.
* **Sécurité des Données :** Chiffrer les données sensibles "au repos" (disques durs) et "en transit" (TLS).
* **Maintenance :** Appliquer les correctifs de sécurité (patching) sous 48h pour les vulnérabilités critiques.

### 4. DETECT (Détecter)

**Importance :** Plus une intrusion est détectée tôt, moins elle coûte cher. Cette fonction vise la visibilité continue.

**Exemples d'actions :**

* **Surveillance Continue :** Déployer un outil de type EDR (Endpoint Detection and Response) sur tous les postes.
* **Analyse des Journaux :** Centraliser les logs de sécurité dans un SIEM pour corréler les événements suspects.
* **Détection d'Anomalies :** Configurer des alertes en cas de connexion inhabituelle (ex: connexion d'un employé à 3h du matin depuis un pays étranger).

### 5. RESPOND (Répondre)

**Importance :** Savoir quoi faire quand l'incident survient pour contenir la menace.

**Exemples d'actions :**

* **Planification de la Réponse :** Rédiger des "Playbooks" (procédures) pour les scénarios courants (Ransomware, vol de PC).
* **Communication :** Établir une liste de contacts d'urgence (juridique, technique, relations presse, autorités).
* **Analyse :** Isoler immédiatement un serveur infecté du reste du réseau pour stopper la propagation.
* **Atténuation :** Réinitialiser les mots de passe de tous les comptes compromis lors d'une attaque.

### 6. RECOVER (Restaurer)

**Importance :** Assurer la résilience de l'entreprise et le retour à la normale.

**Exemples d'actions :**

* **Plan de Continuité :** Tester la restauration des sauvegardes critiques au moins une fois par trimestre.
* **Communication de Reprise :** Informer les clients et partenaires de la disponibilité des services après un incident.
* **Leçons Apprises :** Organiser une réunion "Post-Mortem" après chaque incident majeur pour améliorer les défenses futures.

---

## Comparaison Rapide entre ISO 27001 et NIST CSF 2.0

| Point de comparaison | ISO 27001:2022 | NIST CSF 2.0 |
| :--- | :--- | :--- |
| **Nature** | Norme internationale certifiable. | Cadre de référence (Framework) volontaire. |
| **Approche** | Basée sur la conformité et le management. | Basée sur les résultats et la flexibilité. |
| **Structure** | Clauses (4-10) + 93 Contrôles. | 6 Fonctions -> Catégories -> Sous-catégories. |
| **Public** | Organisations cherchant une preuve de confiance. | Toutes organisations (PME à Grands Groupes). |

---
