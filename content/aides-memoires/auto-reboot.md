---
title: "Redemarrage automatique Linux"
summary: "Mise en place d'un redemarrage automatique sur une machine Linux."
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

Vous pouvez également ajouter d'autre commande à faire avec le démarrage :

```sh
#!/bin/bash
apt update -y   # Vérifie si des mises à jours sont disponibles
apt upgrade -y  # Met les packets à jour
reboot          # Redémarre le machine une fosi les mises à jours terminées
```

Vosu devez ensuite rendre le script executable :

```sh
sudo chmod +x /usr/local/bin/reboot.sh
```

---

## Automatisation du redémarrage

Pour finir nous devons automatiser l'éxécution du script.

Ouvrez l'ordonnanceur `Cron` :

```sh
crontab -e
```

Ensuite ajoutez une ligne pour l'éxéction du script :

```txt
0 23 * * 0 /usr/local/bin/reboot.sh
```

Ici le redémarrage se fait tous les dimanches à 23h. La fréquence est à ajuster selon ce que vous cherchez.

---
