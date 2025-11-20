---
title: "Connexion SSH avec clés RSA"
summary: "Tutoriel pour mettre en place une connexion SSH sécurisée par clés RSA vers un appareil."
date: "2025-10-18"
categories: ["tuto"]
tags: ["linux", "shell", "ssh", "rsa"]
layout: "page"
draft: "false" # Set to true if this page is not to be show
---

---

## Connexion SSH

### Installation d'OpenSSH

Pour commancer il faut installer le serveur ssh sur la machine cilbe.  
Nous allons utiliser *OpenSSH* pour ce tutoriel.

```sh
sudo apt update                  # Met à jours le gestionaire apt
sudo apt install openssh-server  # Installe OpenSSH
sudo systemctl start ssh         # Démarre OpenSSH
sudo systemctl enable ssh        # Ajoute xrdp aux démarages auto
sudo systemctl status ssh        # Affiche le statut de Open SSH
```

Ensuite vérifiez que le port utilisé par les connexions SSH est bien ouvert.

```sh
sudo ufw allow ssh  # Exemple avec le fire wall UFW
```

### Connexion à la machine cible

Pour vous connecter à la machine cible, vous avez besoin de son adresse ip ainsi que du nom de l'utilisateur auquel vous voulez vous connecter.

Pour avoir l'adresse ip, entrez la commande suivante dans votre machine cible.

```sh
ip -br a
```

Une fois que vous avez l'adresse, vous pouvez vous connecter en utilsant la commande suivante sur votre machine source.

```sh
ssh nom_utilisateur@adresse_ip
```

- `nom_utilisateur` correspond à la session avec laquelle vosu allez être connecté
- `adresse_ip` est l'adresse de la machine cible obtenue avec la commande précédente

Lors de la première connexion, un message vous demandera de vérifier l'authenticité de l'hôte en acceptant son empreinte de clé ("fingerprint").

Pour compléter la connexion, vous devez entrer le mot de passe de l'utilisateur avec lequel vouv vous connectez et vous aurrez accès à un shell relié à votre machine cible.

Vous pouvez vous déconnecter en tapant `exit` dans le terminal.

---

## Mise en place des clés RSA

En suivant les étpes précedentes, vosu avez mis en place une connexion ssh entre deux machines. L'étape suivant est de sécuriser la connexion.

Pour ce faire nosu allons mettre en place une connexion par clé RSA. Lors de la connexion SSH, le serveur ne va plus vous demander le mot de passe de l'utilisateur utilisé pour la connexion mais va plutôt regarder si vous possédez la clé privée correspondante à la clé publique qui est enregistrée.

### Générer les clés

La première étape pour sécuriser la connexion est de générer la parie de clé avec la commande suivante

```sh
ssh-keygen -t rsa -b 4096
```

- `ssh-keygen` : outil utilisé piur la génération des clés
- `-t rsa` : force l'utilisation de l'algorithme RSA
- `-b 4096` fixe la taille de la clé à 4096 bits

Une fois la commande entrée, suivez les instructions à l'écran :

- `Enter file in which to save the key (...)` : Entrez l'emplacement de sauvegarde de vos clés. Laissez vide pour l'emplacement par défaut (C:\Users\VotreNom\\.ssh\id_rsa)
- `Enter passphrase (empty for no passphrase)` : Entrez la phrase de passse pour vos clés. Cette phrase vosu sera demandée à chaque utilsiation de la clé. Laissez vide pour ne pas utiliser de phrase de passe.

### Copie de la clé publique sur la machine cible

Maintenat que les clés sont générées, vosu devez copier la clé publique sur le serveur SSH.

Vous pouvez affciher votre clé publique avec la commande suivante (le chemin est à adapter selon votre arborescence)

```sh
cat ~/.ssh/id_rsa.pub
```

La clé est une longue chaine de caractères commençant par `ssh-rsa`.

Une fois la clé affichée, vous pouvez la copier et vous connecter sur votre machine serveur en utilisant le mot de passe de l'utlisateur

```sh
ssh nom_utilisateur@adresse_ip
```

Une fois connecté, vosu devez ajouter votre clé publique sur le serveur ssh (remplacez la partie `public_key` par votre clé publique copiée précédament)

```sh
mkdir -p ~/.ssh && chmod 700 ~/.ssh && echo "public_key" >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys

```

- `mkdir -p ~/.ssh` : Crée le répertoire ~/.ssh s’il n’existe pas.

- `chmod 700 ~/.ssh` : Restreint l’accès au répertoire. Seul l’utilisateur peut lire, écrire, exécuter.

- `echo "COPIEZ_VOTRE_CLÉ_PUBLIQUE_ICI" >> ~/.ssh/authorized_keys` : Ajoute la clé publique au fichier authorized_keys en l’insérant à la fin.

- `chmod 600 ~/.ssh/authorized_keys` : Limite l’accès au fichier aux seules lectures/écritures de l’utilisateur.

Vous pouvez maintenant vous déconnecter de la machine cible.

### Test de la connexion par clé

Pour tester la bonne mis en place des clés RSA, vous pouvez vous connecter comme précédament et cette fois il vous sera demander la phrase de passe que vosu avez définit lors de la génération des clés pour valider votre connexion SSH.

### Sécurisation

Une fosi que vous êtes sur que la connexion avec clés RSA fonctionne, vous pouvez désactiver la connexion par mot de passe.  
Cette démarche assur que seul les appareil ayant une clé RSA reconnue peuvent se connecter à la machine cible.

Pour ce faire, connectez-vous à la machine puis éditez le fichier de configuration du serveur SSH

```sh
sudo nano /etc/ssh/sshd_config
```

Dans ce fichier cherchez les lignes `PasswordAuthentication` et `PubkeyAuthentication` puis modifiez leur valeur par réspéctivment `no` et `yes`.

Au final vous devez avoir quelque chose comme ca

```sh
PasswordAuthentication no
PubkeyAuthentication yes
```

Une fois le fichier enregistré, vous pouvez relancer le serveur SSH pour appliquer les changements.

```sh
sudo systemctl restart ssh
```

Votre serveur n'accepte maintenant que les connexion d'appareil ayant une clé correspondante à l'une de celles enregistrées.

---
