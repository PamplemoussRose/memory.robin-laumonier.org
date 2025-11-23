---
title: "Mise en place d'un dashboard Grafana"
summary: "Tutoriel pour mettre en place un dashboard avec Grafana pour l'interface, Prometheus pour la base de données et Node Exporter pour la collectes des métriques."
date: "2025-11-22"
categories: ["tuto"]
tags: ["linux", "dashboard", "grafana", "prometheus", "node exporter"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Architecture

La mis en place du tableau de bord repose sur plusieurs éléments :

- Node Exporter, le collecteur de métriques
- Prometheus, la base de données
- Grafana, l'interface web

Le collecteur va aller chercher les données système et les transmettre à la base de données.

La base de données va récupérer les métriques et les formater pour en faire des séries temporelles affichables par l'interface.

L'interface va aller chercher les séries créées par la base de données pusi les afficher sous forme de graphiques pour les rendre compréhensibles.

---

## Installation de Node Exporter

### Téléchargement de Node Exporter

Node Exporter est disponible en deux versions selon le type d'architecture que vous avez. Pour ce tutoriel, nous allons utiliser la version 64 bits.

```sh
# Télécharger la version 64 bits
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.1/node_exporter-1.8.1.linux-arm64.tar.gz

# Décompresser l'archive
tar xvfz node_exporter-1.8.1.linux-arm64.tar.gz

# Déplacer l'exécutable
sudo mv node_exporter-1.8.1.linux-arm64/node_exporter /usr/local/bin/

# Nettoyer
rm -rf node_exporter-1.8.1.linux-arm64.tar.gz node_exporter-1.8.1.linux-arm64
```

### Création de l’utilisateur et du service

Ensuite nous devons créer l'utilisateur et le service Node Exporter :

```sh
# Créer l'utilisateur
sudo useradd -rs /bin/false node_exporter

# Créer le fichier de service
sudo nano /etc/systemd/system/node_exporter.service
```

Dans le fichier de service, inserez le code suivant :

```service
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

### Démarrage du service Node Exporter

Une fois le fichier completé, vous pouvez démarrer le service et vérifier qu'il est bien actif :

```sh
sudo systemctl daemon-reload          # Rafraichi la liste des services
sudo systemctl enable node_exporter   # Ajoute node_exporter au démarrage automatique
sudo systemctl start node_exporter    # Lance node_exporter

sudo systemctl status node_exporter   # Affiche le statut de node_exporter
```

Si vous voyez la ligne `Active: active (running)`, le service est correctement lancé.

Node Exporter est maintenant accessible sur le port `9100`

---

## Installation de Prometheus

### Téléchargement de Prometheus

Pour télécherger Prometheus et préparer l'environement, éxecutez les commandes suivantes :

```sh
# Télécharger Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.52.0/prometheus-2.52.0.linux-arm64.tar.gz

# Décompresser l'archive
tar xvfz prometheus-2.52.0.linux-arm64.tar.gz

# Déplacer les exécutables et créer les répertoires
sudo mv prometheus-2.52.0.linux-arm64/prometheus /usr/local/bin/
sudo mv prometheus-2.52.0.linux-arm64/promtool /usr/local/bin/
sudo mkdir /etc/prometheus
sudo mv prometheus-2.52.0.linux-arm64/consoles /etc/prometheus
sudo mv prometheus-2.52.0.linux-arm64/console_libraries /etc/prometheus
sudo mkdir /var/lib/prometheus

# Nettoyer
rm -rf prometheus-2.52.0.linux-arm64.tar.gz prometheus-2.52.0.linux-arm64
```

### Création de l’utilisateur et des permissions

Nous devons ensuite créer un utilisateur pour Prometheus ainsi que lui donner les permissions sur les dossier que nous avons juste créé.

```sh
# Créer l'utilisateur
sudo useradd -rs /bin/false prometheus

# Modifier les permissions
sudo chown -R prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /var/lib/prometheus
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
```

### Configuration de Prometheus

Une fois l'utilisateur créer et les permissions données, nous pouvons créer le fichier de configuration :

```sh
sudo nano /etc/prometheus/prometheus.yml
```

Ce fichier contient le code suivant qui va configurer Prometheus pour qu'il se surveille mais également le Node Exporter.

```yml
global:
  scrape_interval: 15s # Interval de recupération des données. Par défaut 15 secondes.

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']
```

### Création du service

Pour terminer, nosu devons créer le fichier service de Prometheus.

```sh
# Créer le fichier de service
sudo nano /etc/systemd/system/prometheus.service
```

Dans le fichier, inserez le code suivant :

```service
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries \
    --web.listen-address=0.0.0.0:9090

[Install]
WantedBy=multi-user.target
```

### Démarrage du service Prometheus

Une fois le fichier completé, vous pouvez démarrer le service et vérifier qu'il est bien actif :

```sh
sudo systemctl daemon-reload       # Rafraichi la liste des services
sudo systemctl enable prometheus   # Ajoute prometheus au démarrage automatique
sudo systemctl start prometheus    # Lance prometheus

sudo systemctl status prometheus   # Affiche le statut de prometheus
```

Si vous voyez la ligne `Active: active (running)`, le service est correctement lancé.

Prometheus est maintenant accessible sur le port `9090`

---

## Installation de Grafana

### Installation

#### Ajout du dépôt Grafana et installation

Le dépot où se trouve Grafana n'est pas de base dans les dépot `apt`. Nous devons donc l'ajouter avant de pouvoir télécharger Grafana.

```sh
# Télécharger la clé GPG
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /usr/share/keyrings/grafana.gpg > /dev/null

# Ajouter le dépôt pour les architectures ARM (Raspberry Pi)
echo "deb [signed-by=/usr/share/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list

# Mettre à jour les dépots et installer Grafana
sudo apt update
sudo apt install grafana -y
```

#### Démarrage du service Grafana

```sh
sudo systemctl daemon-reload          # Rafraichi la liste des services
sudo systemctl enable grafana-server  # Ajoute grafana-server au démarrage automatique
sudo systemctl start grafana-server   # Lance grafana-server

sudo systemctl status grafana-server  # Affiche le statut de grafana-server
```

Si vous voyez la ligne `Active: active (running)`, le service est correctement lancé.

Grafana est maintenant accessible sur le port `3000`

### Configuration

#### Connexion

Vous pouvez acceder à l'interface Grafana via l'URL <http://localhost:3000>.

Lors de la primère connexion sur Grafana, les identifiants par défaut sont :

- Utilisateur : admin
- Mot de passe : admin

Il vus sera alorts demandé de changer le mot de passe pour les futures connexions.

#### Lien avec Prometheus

Une fois connecté, vous devez lier grafana avec Prmoetheus.

Pour ce faire, allez dans la partie *Connection* puis cliquez sur *Data sources*.

Vous aurez alors un bouton *Add new data source*. Cliquez dessus et séléctionnez *Prometheus*.

Une fois sur la page de configuration du datasource, choissez le nom et entrez `http://localhost:9090` dans le champs *Prometheus server URL*. Vous pouvez ensuite aller en bas de la page et cliquer sur bouton *Save & test*. Si le test est validé, Grafana est correctement lié à Prometheus.

#### Importation du tableau de bord

Pour le tableau de bord, vous pouvez en créer un par vous-même. Cependant, nous allons ici en importer un déjà existant.

Pour commencer allez dans l'onglet *Dashboard* puis cliquez sur *New* et choisissez *New dashboard*.

Une fois dans le menu, choissiez *Import a dashboard*, entrez `1860` dans le champs *Grafana.com dashboard URL or ID* puis cliquez sur *Load*.

Pour finir, choissiez le nom du tableau de bord et cliquez sur *Import*. Vous trouverez alors votre tableau de bord dans l'onglet *Dashboard* ou depuis l'accès rapide de l'onglet *Home*.

Le tableau de bord importer est le suivant :

![Dashbord Grafana](/images/dashboard.png)

---
