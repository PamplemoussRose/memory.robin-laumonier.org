---
title: "Commandes Shell basiques"
summary: "Toutes les commandes de base pour utiliser le shell linux."
date: "2025-10-08"
categories: ["mémo"]
tags: ["linux", "shell", "commandes"]
layout: "page"
draft: "false" # Set to true if this page is not to be show
---

---

## Déplacement

```sh
cd path
```

Description :  
Change le repertoir courrant par celui de l'adresse donnée.

---

## Listing

```sh
ls [ OPTION ] path
```

Description :  
Liste les éléments présents dans le dossier donné.  
Si *path* est vide, affiche les éléments du repoertoir courrant.

Options :

- `-a` : Affiche également les éléments cachés (nom commence par '.')
- `-l` : Affiche le détail des éléments (permissions, propriétaire, taille, date de modification)
- `-h` : Quand le détail est affiché, met les tailles en unité lisible
- `-1` : Affiche 1 élément par ligne
- `-R` : Scan les dossiers récursivement

---

## Clear

```sh
clear
```

Description :  
Nettoie l'interface du terminal.

---

## Adresse IP

### Adresse publique du routeur

```sh
curl [ OPTION ] ifconfig.me
```

Description :  
Affiche l'adresse IP publique du routeur.

Options :

- `-4` : Affiche l'adresse IPv4
- `-6` : Affiche l'adresse IPv6

### Adresse local de l'appareil

```sh
ip [ OPTION ] a [ OBJET ]
```

Description :  
Affiche les interfaces et leurs adresses.

Options :

- `-br` : Affiche le resultat en format court
- `-4` : Affiche les interfaces avec des adresses IPv4
- `-6` : Affiche les interfaces avec des adresses IPv6

Objets :

- `show <interface>` : Affiche l'interface indiquée
- `show to <adresse>` : Affiche l'interface qui utilise l'adresse
- `show up` : Affiche uniquement les interfaces actives

---

## Copier

```sh
cp [ OPTION ] source cible
```

Description :  
Fait une copie d'un fichier ou d'un dossier source dans le repertoir cible.

Options :

- `-v` : Sortie avec verbose
- `-u` : Copie uniquement si le fichier source est plus récent
- `-i` : Demande une confirmation avant d'écraser
- `-r` : Fait une copie récursive (pour les dossiers)

---

## Supprimer

```sh
rm [ OPTION ] path
```

Description :  
Supprime les éléments à l'adresse donnée.

Options :

- `-f` : Force la suppression
- `-i` : Demande confirmation à chaque suppression
- `-r` : Supprime recursivement tous les éléments à partir de l'adresse
- `-d` : Supprime les dossiers vides
- `-v` : Affiche les éléments supprimés

---

## Permissions

```sh
chmod [ OPTION ] [ PERM ] path
```

Description :  
Change les permissions de l'élément donné.

Options :

- `-R` : Applique les permissions récursivement
- `-v` : Affiche les éléments modifiés
- `-c` : Affiche uniquement les changements effectués

Permissions :  
Les droits sont données selon 3 chiffres &rarr; XXX.  
Chaque chiffre est compris entre 0 et 7 et est la somme de 1 pour le droit d'exécution (x), 2 pour le droit d'écriture (w) et 4 pour le droit de lecture (r).

- Le premier chiffre est pour le propriétaire
- Le second est pour le groupe
- Le troisième est pour les autres utilisateur

---

## Propriété

```sh
chown [ OPTION ] [ USER ] path
```

Description :  
Change le propriétaire de l'élément donné.

Options :

- `-R` : Change récursivement le propriétaire et/ou le groupe des fichiers
- `-v` : Affiche les fichiers dont le propriétaire ou le groupe a été modifié
- `-c` : Affiche uniquement les fichiers modifiés

Utilisateur :

- `user` : change le propriétaire
- `user:groupe` : change le propriétaire et le groupe

---

## Emplacement actuel

```sh
pwd
```

Description :  
Affiche le chemin du dossier courrant.

---

## Redémarrer

```sh
sudo reboot
```

---
