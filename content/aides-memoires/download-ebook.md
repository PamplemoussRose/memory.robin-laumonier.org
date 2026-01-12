---
title: "Téléchargement d'e-book en PDF"
summary: "Tutoriel pour télécharger des e-books depuis O'Reilly et les convertir en PDF"
date: "2026-01-11"
categories: ["tuto"]
tags: ["ebook", "pdf"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Prérequis

Pour ce tutoriel, vous devez avoir :

- git installé
- python installé
- un compte O'Reilly valide

---

## Téléchargement de l'e-book

1. Connectez-vous à votre compte O'Reilly

2. Allez sur la page du livre que vous voulez télécharger et repérez l'ID du livre.  
    Exemple : learning.oreilly.com/library/view/enterprise-security-architecture/**9781578203185**/

3. Sur cette même page, ouvrez l'inspecteur, allez dans l'onglet `Stockage` puis copiez les valeurs des cookies `_abck`, `bm_sz`, `orm-jwt` et `orm-rt` pour les enregistrer dans un fichier `cookies.json`.

4. Clonez le repo git [https://github.com/lorenzodifuccia/safaribooks.git](https://github.com/lorenzodifuccia/safaribooks.git) avec la commande suivante :

    ```sh
    git clone https://github.com/lorenzodifuccia/safaribooks.git
    ```

5. Copiez ou déplacez le fichier `cookies.json` à la racine du repo.

    Vous devez alors avoir l'arborescence suivante :

    ```txt
    /safaribooks
    ├── .gitignore
    ├── cookies.json
    ├── LICENSE.md
    ├── Pipfile
    ├── README.md
    ├── requirements.txt
    ├── retrieve_cookies.py
    └── safaribooks.py
    ```

6. Déplacez-vous dans le dossier et installez les dépendances pour le script python :

    ```sh
    cd safaribooks
    pip3 install -r requirements.txt
    ```

7. Pour finir, exécutez le script python en passant l'ID du livre en paramètre de la commande :

    ```sh
    python3 safaribooks.py 9781578203185
    ```

    Si le script ne rencontre pas d'erreurs, un dossier `Books` sera créé à la racine du repo.  
    Ce dossier contient tous les répertoires des livres téléchargés via le script.

    Exemple de sortie sans erreurs :

    ```txt
     ██████╗     ██████╗ ██╗  ██╗   ██╗██████╗
    ██╔═══██╗    ██╔══██╗██║  ╚██╗ ██╔╝╚════██╗
    ██║   ██║    ██████╔╝██║   ╚████╔╝   ▄███╔╝
    ██║   ██║    ██╔══██╗██║    ╚██╔╝    ▀▀══╝
    ╚██████╔╝    ██║  ██║███████╗██║     ██╗
     ╚═════╝     ╚═╝  ╚═╝╚══════╝╚═╝     ╚═╝

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    [-] Successfully authenticated.
    [*] Retrieving book info...
    [-] Title: Enterprise Security Architecture
    [-] Authors: Nicholas Sherwood
    [-] Identifier: 9781578203185
    [-] ISBN: 9781498759908
    [-] Publishers: CRC Press
    [-] Rights:
    [-] Description: Security is too important to be left in the hands of just one department or employee-it's a concern of an entire enterprise. Enterprise Security Architecture shows that having a comprehensive plan requires more than the purchase of security software-it requires a framework for developing and maintaining a system that is proactive. The book is based
    [-] Release Date: 2005-11-15
    [-] URL: https://learning.oreilly.com/library/view/enterprise-security-architecture/9781578203185/
    [*] Retrieving book chapters...
    [*] Output directory:
        safaribooks\Books\Enterprise Security Architecture (9781578203185)
    [-] Downloading book contents... (33 chapters)
        [##################################################################################] 100%
    [-] Downloading book CSSs... (3 files)
        [##################################################################################] 100%
    [-] Downloading book images... (142 files)
        [##################################################################################] 100%
    [-] Creating EPUB file...
    [*] Done: safaribooks\Books\Enterprise Security Architecture (9781578203185)\9781578203185.epub

        If you like it, please * this project on GitHub to make it known:
            https://github.com/lorenzodifuccia/safaribooks
        e don't forget to renew your Safari Books Online subscription:
            https://learning.oreilly.com

    [!] Bye!!
    ```

    Votre e-book se trouve dans le dossier portant le nom du livre et est nommé avec l'ID de celui-ci.  
    Exemple : `safaribooks\Books\Enterprise Security Architecture (9781578203185)\9781578203185.epub`

---

## Conversion en PDF

Pour convertir votre e-book en fichier PDF, il suffit d'utiliser un convertisseur en ligne comme [https://www.freepdfconvert.com](https://www.freepdfconvert.com).

---

## Avertissement légal

Ce tutoriel est fourni uniquement à des fins pédagogiques et informatives.

L’utilisation des outils et méthodes décrits ci-dessus doit se faire exclusivement dans le respect des conditions générales d’utilisation (CGU) d’O’Reilly ainsi que des lois en vigueur relatives au droit d’auteur et à la propriété intellectuelle.

En particulier :

- Vous devez disposer d’un compte O’Reilly valide et actif.
- Les contenus téléchargés doivent être utilisés uniquement pour un usage personnel et privé.
- Toute redistribution, mise à disposition publique, partage ou usage commercial des ouvrages téléchargés est strictement interdit sans l’autorisation explicite des ayants droit.
- Vous êtes seul responsable de l’utilisation que vous faites des informations et outils présentés dans ce tutoriel.

L’auteur de ce tutoriel ne saurait être tenu responsable d’un usage non conforme, abusif ou illégal des techniques décrites.

---
