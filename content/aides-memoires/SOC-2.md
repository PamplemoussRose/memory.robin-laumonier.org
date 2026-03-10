---
title: "Rapport SOC 2"
summary: "Guide sur le Rapport SOC 2 avec explication de chaque critères et exemples d'actions pour chacune d'eux."
date: "2026-03-10"
categories: ["mémo"]
tags: ["cyber-secu", "norme", "soc"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

Le **SOC 2 (System and Organization Controls 2)** est un cadre d'audit développé par l'AICPA (American Institute of Certified Public Accountants). Le SOC 2 est un **rapport d'attestation** délivré par un cabinet d'audit tiers (CPA) qui évalue la manière dont une entreprise de services gère les données de ses clients.

Il est particulièrement crucial pour les entreprises **SaaS, Cloud et de services technologiques**.

---

## Les 5 Critères de Service de Confiance (Trust Services Criteria - TSC)

Le SOC 2 s'appuie sur cinq piliers. Seul le critère "Sécurité" est obligatoire ; les autres sont choisis en fonction de la pertinence pour le service rendu.

### 1. Sécurité (Security / Common Criteria) - *Obligatoire*

**Importance :** C'est le socle du rapport. Il garantit que les systèmes sont protégés contre les accès non autorisés, les dommages physiques et les modifications qui pourraient compromettre les autres critères.

**Exemples d'actions :**

* **Contrôle d'accès :** Mise en œuvre de l'authentification multi-facteurs (MFA) et révision trimestrielle des accès privilégiés.
* **Protection réseau :** Utilisation de pare-feu (WAF) et de systèmes de détection d'intrusion (IDS/IPS).
* **Gestion des vulnérabilités :** Scans de vulnérabilités mensuels et tests d'intrusion (Pentest) annuels.

### 2. Disponibilité (Availability)

**Importance :** Ce critère évalue si le système est disponible pour l'utilisation conformément aux engagements (SLA). Il ne s'agit pas seulement de "uptime", mais de la capacité à maintenir le service.

**Exemples d'actions :**

* **Surveillance (Monitoring) :** Mise en place d'alertes en temps réel sur les performances des serveurs et la bande passante.
* **Plan de Reprise d'Activité (DRP) :** Documentation et test annuel d'un plan de basculement vers un site de secours.
* **Sauvegardes :** Automatisation des sauvegardes avec tests de restauration réguliers pour garantir l'intégrité des données récupérées.

### 3. Intégrité du Traitement (Processing Integrity)

**Importance :** Garantit que le traitement des données est complet, valide, exact, opportun et autorisé. C'est essentiel pour les services financiers ou de transaction de données.

**Exemples d'actions :**

* **Validation des entrées :** Contrôles applicatifs pour empêcher l'injection de données erronées ou malveillantes.
* **Suivi des erreurs :** Journalisation de toutes les erreurs de traitement avec un processus formel de résolution.
* **Contrôle de version :** Utilisation de Git avec revue de code obligatoire avant tout déploiement en production.

### 4. Confidentialité (Confidentiality)

**Importance :** Se concentre sur la protection des données considérées comme confidentielles par contrat ou par politique interne (ex: secrets commerciaux, propriété intellectuelle).

**Exemples d'actions :**

* **Chiffrement :** Chiffrement des données sensibles au repos (AES-256) et en transit (TLS 1.2+).
* **Accords de confidentialité (NDA) :** Signature systématique de clauses de confidentialité par tous les employés et sous-traitants.
* **Classification des données :** Marquage clair des documents confidentiels et restriction d'accès basée sur le principe du "moindre privilège".

### 5. Confidentialité des Données Personnelles (Privacy)

**Importance :** À ne pas confondre avec la "Confidentialité" ci-dessus. Ce critère concerne spécifiquement les **données personnelles (PII)** et leur collecte, utilisation, conservation et divulgation conformément aux lois (RGPD, CCPA).

**Exemples d'actions :**

* **Politique de confidentialité :** Publication d'une notice d'information claire sur la collecte des données.
* **Consentement :** Mise en place de mécanismes pour recueillir et gérer le consentement des utilisateurs.
* **Droit à l'oubli :** Procédure formelle pour supprimer les données personnelles d'un client sur simple demande.

---

## SOC 2 Type I vs. SOC 2 Type II

| Caractéristique | SOC 2 Type I | SOC 2 Type II |
| :--- | :--- | :--- |
| **Période** | À un instant T (date précise). | Sur une période (ex: 6 ou 12 mois). |
| **Objectif** | Évalue la **conception** des contrôles. | Évalue l'**efficacité opérationnelle**. |
| **Valeur** | Preuve rapide de mise en place. | Preuve de maturité et de rigueur. |
| **Effort** | Moins coûteux et plus rapide. | Exige des preuves continues (logs, tickets). |

---
