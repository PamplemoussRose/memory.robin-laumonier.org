---
title: "Téléchargement d'ebook en pdf"
summary: "Tutoriel pour télécharger des ebook depuis O'Reilly et les convertir en pdf"
date: "2026-01-11"
categories: ["tuto"]
tags: ["ebook", "pdf"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Prérequis

Pour ce tutoriel, vous devez avoir :

- git d'installé
- python d'installé
- un compte O'Reilly valide

---

## Téléchargement de l'ebook

1. Connectez-vous à votre compte O'Reilly

2. Allez sur la page du livre que vous voulez télécharger et repérez l'ID du livre.  
    Exemple : learning.oreilly.com/library/view/enterprise-security-architecture/**9781578203185**/

3. Sur cette même page, ouvrez l'inspecteur, allez dans l'onglet `Stockage` puis copiez les valeurs des cookies `_abck`, `bm_sz`, `orm-jwt` et `orm-rt` pour les enregistrer dans un fichier `cookies.json`.

4. Clonnez le repo git [https://github.com/lorenzodifuccia/safaribooks.git](https://github.com/lorenzodifuccia/safaribooks.git) avec la commande suivante :

    ```sh
    git clone https://github.com/lorenzodifuccia/safaribooks.git
    ```

5. Copiez ou déplacez le fichier `cookies.json` à la racine du repo

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

6. Déplacez vous dans le dossier et installez les dépendances pour le script python :

    ```sh
    cd safaribooks
    pip3 install -r requirements.txt
    ```

7. Pour finir, éxécutez le script pyhton en passant l'ID du livre en paramêtre de la commande :

    ```sh
    python3 safaribooks.py 9781578203185
    ```

    Si le script ne rencontre pas d'erreurs, un dossier `Books` sera créé à la racine du repo.  
    Ce dossier contient tous les dossiers des livres téléchargés via le script.

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

    Votre e-book est donc dans le dossier ayant le nom du livre et est nommé avec l'ID de celui-ci.  
    Exemple : `safaribooks\Books\Enterprise Security Architecture (9781578203185)\9781578203185.epub`

---

## Conversion en PDF

Pour convertir votre e-book en fichier pdf, il suffit d'utiliser un convertisseur en ligne comme [https://www.freepdfconvert.com](https://www.freepdfconvert.com).

---
