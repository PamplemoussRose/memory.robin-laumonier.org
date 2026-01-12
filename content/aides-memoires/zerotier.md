---
title: "Mise en place de ZeroTier"
summary: "Tutoriel pour mettre en place ZeroTier."
date: "2025-11-24"
categories: ["tuto"]
tags: ["vpn", "réseau privé", "windows", "linux"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Installation

### Windows

Pour installer ZeroTier sous Windows, allez à l'adresse suivante, téléchargez le fichier d'installation puis exécutez le.

<https://www.zerotier.com/download/>

### Linux

Pour installer ZeroTier sous Linux, entrez la commande suivante :

```sh
curl -s https://install.zerotier.com | sudo bash
```

---

## Configuration

### Création du réseau

Pour créer un nouveau réseau sur ZeroTier, vous devez aller sur l'interface d'administration en ligne ZeroTier (<https://my.zerotier.com/network>).

Ensuite cliquez sur *Create A Network*. Un nouveau réseau apparaîtra et vous aurez le code à 16 chiffres unique à ce réseau qui vous permet de le rejoindre réseau.

### Rejoindre un réseau sous Windows

Affichez les icônes cachées puis cliquez sur ZeroTier.

Sélectionnez *Join New Network...* et entrez le code à 16 chiffres de votre réseau créé précédemment.

### Rejoindre un réseau sous Linux

Entrez la commande suivante :

```sh
sudo zerotier-cli join <network ID>
```

- `<network ID>` est le code à 16 chiffres de votre réseau créé précédemment.

### Accepter un nouvel appareil

Quand un nouvel appareil rejoint votre réseau, il va être *Not authorized*.

Pour l'authoriser à accéder au réseau, vous devez vous rendre sur l'interface d'administration de votre réseau. Ensuite allez dans le détail de l'appareil à autoriser puis cochez la case *Authorized* et cliquez sur *Save*.

Votre appareil pourra alors communiquer avec les autres machines de votre réseau via les adresses ZeroTier.

---

## Commandes utiles

### Afficher l'aide

```sh
sudo zerotier-cli -h
```

### Afficher la version

```sh
sudo zerotier-cli -v
```

### Rejoindre un réseau

```sh
sudo zerotier-cli join <network ID>
```

`<network ID>` est le code à 16 chiffres du réseau

### Quitter un réseau

```sh
sudo zerotier-cli leave <network ID>
```

`<network ID>` est le code à 16 chiffres du réseau

### Afficher la liste des réseaux rejoints

```sh
sudo zerotier-cli listnetworks
```

### Afficher un paramètre d'un réseau

```sh
sudo zerotier-cli get <network ID> <setting> #      
```

`<network ID>` est le code à 16 chiffres du réseau

`<setting>` doit avoir une des valeurs suivantes :

- `nwid` : code à 16 chiffres du réseau
- `name` : nom du réseau
- `mac` : adresse mac
- `status` : statut du réseau
- `type` : accessibilité du réseau
- `dev` : nom de l'interface réseau
- `ZT assigned ips` : ip de la machine sur le réseau
