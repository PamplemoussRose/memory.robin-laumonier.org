---
title: "Redémarrage automatique Linux"
summary: "Mise en place d'un redémarrage automatique sur une machine Linux."
date: "2025-12-19"
categories: ["tuto"]
tags: ["linux", "shell"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Script de redémarrage

Créez un nouveau script bash :

```sh
sudo nano /usr/local/bin/reboot.sh
```

Ajoutez le code suivant :

```sh
#!/bin/bash
reboot
```

Vous pouvez également ajouter d'autre commandes à faire avec le démarrage :

```sh
#!/bin/bash
apt update -y   # Met à jour la liste des paquets
apt upgrade -y  # Met les paquets à jour
reboot          # Redémarre la machine une fois les mises à jours terminées
```

Vous devez ensuite rendre le script exécutable :

```sh
sudo chmod +x /usr/local/bin/reboot.sh
```

---

## Automatisation du redémarrage

Pour finir, nous devons automatiser l'exécution du script.

### Création du service

Pour commencer, vous devez créer un service qui va éxécuter votre script :

```sh
sudo nano /etc/systemd/system/auto-reboot.service

```

Ensuite ajoutez le code suivant :

```service
[Unit]
Description=Reboot

[Service]
Type=oneshot
ExecStart=/usr/local/bin/reboot.sh
```

La partie `Type=oneshot` indique que le service est à éxécuter une seule fois.  
Vous pouvez également personnaliser la description.

### Création du timer

Une fois le service créé, vous devez créer le timer qui rendera l'éxécution automatique.

```sh
sudo nano /etc/systemd/system/auto-reboot.timer

```

Le code suivant permet de programmer l'exécution tous les dimanches à 23h59. La partie `Persistent=true` indique que l'éxécution se fera au démarrage de la machine si elle est éteinte à la date programmée.

```timer
[Unit]
Description=Reboot schedule

[Timer]
OnCalendar=Sun 23:59
Persistent=true

[Install]
WantedBy=timers.target
```

Il est possible de modifier la description et la date du timer.

---
