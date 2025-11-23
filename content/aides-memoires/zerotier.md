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

<https://www.zerotier.com/download/>

### Linux

```sh
curl -s https://install.zerotier.com | sudo bash
```

---

## Configuration

### Création du réseau

### Rejoindre un réseau sous Windows

### Rejoindre un réseau sous Linux

---

## Commandes utiles

```sh
sudo zerotier-cli -h
sudo zerotier-cli -v
sudo zerotier-cli join <network ID>
sudo zerotier-cli leave <network ID>
sudo zerotier-cli listnetworks
sudo zerotier-cli get <network ID> <setting> #<nwid> <name> <mac> <status> <type> <dev> <ZT assigned ips>
```
