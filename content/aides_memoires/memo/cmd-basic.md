---
title: "Commandes cmd basiques"
summary: "Toutes les commandes de base pour utiliser le cmd Microsoft."
date: "2025-10-09"
categories: ["mémo"]
tags: ["windows", "cmd", "commandes"]
layout: "page"
draft: "false" # Set to true if this page is not to be show
---

---

## Déplacement

```bat
cd path
```

Description :  
Change le repertoir courrant par celui de l'adresse donnée.

---

## Listing

```bat
dir [ OPTION ] path
```

Description :  
Liste les éléments présents dans le dossier donné.  
Si *path* est vide, affiche les éléments du repoertoir courrant.

Options :

- `/A` : Affiche les fichiers avec attributs spécifiques
- `/B` : Affiche uniquement les nom des éléments
- `/S` : Scan recursivment
- `/P` : Pause à chaque page lors de l'affichage

---

## Clear

```bat
cls
```

Description :  
Nettoie l'interface du terminal.

---

## Adresse IP

### Adresse publique du routeur

```bat
curl [ OPTION ] ifconfig.me
```

Description :  
Affiche l'adresse IP publique du routeur.

Options :

- `-4` : Affiche l'adresse IPv4
- `-6` : Affiche l'adresse IPv6

### Adresse local de l'appareil

```bat
ipconfig
```

Description :  
Affiche les interfaces et leurs adresses.

---

## Copier

### Copie de fichier

```bat
copy [ OPTION ] source destination
```

Description :  
Fait une copie du fichier source dans le repertoir cible.

Options :

- `/Y` : Supprime les demandes de confirmation d'écrasement
- `/V` : Vérifie l'intégrité après copie
- `/A` : Copie en mode texte
- `/B` : Copie en mode binaire

### Copie de dossiers

```bat
xcopy [ OPTION ] source destination
```

Description :  
Fait une copie du dossier source dans le repertoir cible.

Options :

- `/E` : Inclut sous-dossiers même vides
- `/I` : Suppose que la destination est un dossier
- `/Y` : Supprime les demandes de confirmation d'écrasement
- `/H` : Inclut fichiers cachés/système
- `/C` : Continue malgré les erreurs

---

## Supprimer

### Suppression de fichiers

```bat
del [ OPTION ] file
```

Description :  
Supprime le fichier à l'adresse donnée.

Options :

- `/F` : Force la suppression des fichiers en lecture seule
- `/S` : Parcour le repertoir courrant recursivement et supprime tous les fichiers nommé *file*
- `/Q` : Supprime les demandes de confirmation
- `/P` : Demande la confirmation à chaque suppression

### Suppression de dossiers

```bat
rd [ OPTION ] directory
```

Description :  
Supprime le dossier à l'adresse donnée.

Options :

- `/S` : Supprime recursivement
- `/Q` : Supprime les demandes de confirmation

---

## Permissions et propriété

```bat
icacls path [ OBJET ] [ OPTION ]
```

Description :  
Affiche les permissions sur l'élément donné.

Options :

- `/T` : Les modifications sont faites sur tout ce qui est enfant du chemin donné
- `/C` : Ignore les messages d'erreur
- `/L` : La modification se fait sur un lien symbolique
- `/Q` : Mode silencieux

### Modification de permissions

Objet :

- `/grant user:perm` : Change les droits de l'utilisateur
- `/remove user` : Retire les droits de l'utilisateur

*perm* peut prendre les valeurs suivantes :

- N - Aucun accès
- F - Accès complet
- M - Accès en modification
- RX - Accès en lecture et exécution
- R - Accès en lecture seule
- W - Accès en écriture seule
- D - Accès en suppression

### Modification de propriétaire

Objet :

- `/setowner user` : Change le propriétaire du fichier

---

## Emplacement actuel

```bat
cd
```

Description :  
Affiche le chemin du dossier courrant.

---

## Redémarrer

```cmd
shutdown /r /f /t 0
```

Explication des options :

- `/r` : redémarre l'ordinateur.
- `/f` : force la fermeture des applications en cours.
- `/t 0` : définit le délai avant le redémarrage à 0 secondes.

---
