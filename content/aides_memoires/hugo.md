---
title: "Tuto Hugo avec Blowfish"
summary: "Tutoriel d'installation et de configuration pour créer un site avec Hugo."
date: "2025-11-18"
categories: ["tuto"]
tags: ["hugo", "blowfish", "markdown", "site web"]
layout: "page"
draft: "false" # Set to true if this page is not to be show
---

---

## Installer Hugo et Git

Hugo est un framework qui permet de créer des sites web statiques à partir de fichier en Marwkdown.  
Pour commencer, il faut donc installer Hugo sur votre machine.

De plus, Hugo permet de personnaliser le visuel de son site en utilisant des thèmes.  
Les thèmes peuvent être construit de zéro ou lié au projet via Git. Dans ce tutoriel, nous importerons le thème Blowfish depuis Git.  
Si vous prévoyez de lier un thème, il faut vérifier que Git est bien installé sur votre machine.

### Installer Hugo

- Sous Windows :

```bash
winget install Hugo.Hugo.Extended
```

- Sous Linux :

```bash
sudo apt install hugo
```

### Installer Git

- Sous Windows :

```bash
winget install --id Git.Git -e --source winget
```

- Sous Linux :

```bash
apt-get install git
```

---

## Initialiser le projet

Une fois que Hugo et Git sont instllés, nous pouvons initialiser le projet via la commande suivante :

```bash
hugo new site path
```

La variable `path` correspond au chemin du dossier ou sera initialisé le projet.

Une fois le projet créé, nosu pouvons nous déplacer dans le dossier. Toutes les commandes qui modifieront le projet seront à faire depuis la racine de celui-ci.

```bash
cd path
```

---

## Configurer le thème

### Installation

Pour stocké le code de notre site, il est recommandé de créer un dépôt Git. De plus, nous allons utiliser Git pour importer le thème Blowfish dans notre projet.

Pour commencer, il faut donc créer un déport Git à la racine de notre projet (`path`) :

```bash
git init
```

Une fois le dépôt initialisé, nous pouvons ajouter le dépôt *blowfish.git* en tant que sous module de notre dépôt :

```bash
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
```

Cette commande va importer le dépôt *blowfish.git* dans le dossier `theme/blowfish` ce qui permet d'avoir accès aux fichiers du thème et depersonnaliser la disposition des éléments si celle d'origine de vous plait pas.

Pour terminer l'installation de Blowfish, il faut :

- Supprimer le fichier `hugo.toml`qui est à la racine du projet
- Copier tous les fichiers *.toml* du dossier `theme/blowfish/config/_default` dans le dossier `config/_default`
- Décommenter la ligne `theme = "blowfish"` dans le fichier `hugo.toml`

Une fois ces étapes réalisées, votre dossier `config/_default` doit ressembler à ça :

``` sh
config/_default/
├─ hugo.toml               # Configuration du site
├─ languages.en.toml       # Paramètres de la langue et de l'auteur
├─ markup.toml             # Paramètres pour quele thème fonctionne
├─ menus.en.toml           # Personnalisation du header et du footer
├─ params.toml             # Personnalisation des élément du site
└─ module.toml
```

Et le fichier `hugo.toml` doit commencer par les lignes suivantes :

```toml
# -- Site Configuration --
# Refer to the theme docs for more details about each of these parameters.
# https://blowfish.page/docs/getting-started/

theme = "blowfish" # UNCOMMENT THIS LINE
```

### Configuration basique

Avant de passer à la personnaisation du site, il reste quelques éléments à régler.

Pour commencer, dans le fichier `hugo.toml`, indiquez l'URL racine de votre site ainsi que la langue du site.  

- `baseURL` correspond à l'adresse de la page d'accueil du site  
- `languageCode` correspond aux deux lettres de la langue utilisée pour écrire le contenu du site

``` toml
# config/_default/hugo.toml

baseURL = "https://domaine.com/"
languageCode = "fr"
```

Une fois la langue choisie, modifiez le code de langue dans le nom des fichiers `languages.en.toml` et `menus.en.toml`.  
Pour un site en français, les fichiers seront nommés `languages.fr.toml` et `menus.fr.toml`.  
Le code utilisé dans les noms des fichiers doit être le même que celui de la variable `languageCode` dans le fichier `hugo.toml`.

---

## Personnalisation

### Informations sur l'auteur

Le fichier `language.en.toml` contient une section concernant les informations de l'auteur du site.  
Pour ajouter une information, il y a juste à décommenter la ligne et remplacer la ligne temporaire pour correspondre à l'information réelle.  
Au contraire, pour qu'une information ne soit pas affichée, il suffit de commenter la ligne de l'information en question.

Dans le cas où vous voulez mettre une photo de profil, l'image doit être stockée dans le dossier `assets/img` et le chemin doit être spécifier dans la ligne correspondante.

Si vous voulez ajouter des liens, veillez à ce que la ligne `links = [` ainsi que la dernière ligne `]` sont bien décommentées.

```toml
[params.author]
   name = "Martin Dupond"
   image = "img/profil.png"
   links = [
     { x-twitter = "https://twitter.com/martindupond" },
   ]
```

### Palette de couleurs

Il est possible de personnaliser les couleurs utilisées par Blowfish.  
Par défaut, le trio `blowfish` est utilisé mais il existe 16 trio de couleurs disponibles.  
Tous les trios sont disponibles à l'adresse suivante : <https://blowfish.page/docs/getting-started/#colour-schemes>

Pour changer le trio utilisé, il faut modifier la viraible `colorScheme` dans le fichier `params.toml`.

```toml
# config/_default/params.toml

colorScheme = "blowfish"
```

Il est également possible de créer son propre trio.  
Pour ce faire, il faut créer un nouveau fichier avec le format `<nom-trio>.css` dans le dossier `assets/css/schemes/` et spécifier les couleurs à utiliser.  
Pour d'informations sur les trios de couleurs, visitez <https://blowfish.page/docs/advanced-customisation/#colour-schemes>

---

## Ajout de pages

### Organisation des pages

Les pages que vous voulez ajouter doivent toutes être dans le dossier `content`. Cependant, il n'y a pas d'organisation spécifique donc l'architecture de votre site dépend de vous.

Une architecture peut être la suivante :

```sh
content
├── _index.md
├── about.md
├── posts
│   ├── _index.md
│   ├── first-post.md
│   └── another-post.md
└── projetcs
    ├── _index.md
    └── projet1.md
```

A chaque niveau de l'architecture, les fichiers `index.md` servent à personnaliser ce qui est affiché sur chaque page.  
Le fichier `content/_index.html` correspond à la page `domaine.com/`, `content/posts/_index.html` à la page `domaine.com/posts`, etc.

### Nouvelles pages

Lors de la création d'une page, il est important de mettres quelques lignes spécifiques au debut des fichiers `.md`. Elles permetent d'avoir nottement le nom de la page, le type de disposition ou encore si la page est à prendre en compte ou non.

```md
---
title: "Tuto Hugo avec Blowfish"
summary: "Tutoriel d'installation et de configuration pour créer un site avec Hugo."
categories: ["tuto"]
tags: ["hugo", "blowfish", "markdown", "site web"]
layout: "page"
draft: "false"
---
```

- `title` : Titre de la page. Il sera afficher en gros au debut de la page ainsi que dans le nom de l'onglet.
- `summary` : Description sommaire du contenu de la page. Il peut être affiché avec le titre de la page dans la liste des pages disponibles si l'option est activée.
- `categories` : Catégorie de la page. Sert de critère de recherche.
- `tags` : Sujets de la page. Sert de critère de recherche.
- `layout` : Type de disposition du contenu à utiliser pour la page. Les dispositions par défaut sont `simple`, `page`, `profile`, `hero`, `card`ou `background`.
- `draft` : Booléen pour savoir si la page doit être ingorée lors du rendu du site. Une page ignorée ne sera pas disponible sur le site final.

Les `categories` et `tags` sont différenciés par la possiblité de n'afficher que les catégories ou d'afficher les catégories et les tags sur les pages.

### Personnalisation des menus

Il est également possible de personnaliser le header et le footer du site.

Pour cela, il faut aller dans le fichier `menus.en.toml` et ajuster les menus comme vosu le souhaitez.

Chaque élément du menu suis l'un des format suivant :

```toml
# Element parent
[[menu]]
  name = "Element parent"
  pageRef = "parent"
  weight = 1

# Element enfant
[[menu]]
  name = "Element enfant"
  parent = "Element parent"
  pageRef = "parent/enfant"
  weight = 1
```

- `[[menu]]` : Peut être `main` ou `footer`. Correspond au menu auquel sera rattaché l'élement.
- `name` : Nom de l'élément. C'est ce qui sera affiché dans le menu.
- `pageRef` : Page de redirection lors du clique de l'élément.
- `weight` : Poids de l'élément dans le menu. Plus il est faible, plus l'élément sera affciher au début du menu.
- `parent` : Pour les éléments enfants, élément auquel il est rattaché.

Si deux éléments sont au même niveau avec le même poids, l'affichage se fait par ordre alphabétique.

Un élément parent sans attribut `pageRef` ne va pas être cliquable mais va se dérouler s'il a des éléments enfant.

---

### Tester le site

Hugo permet de lancer un serveur local pour tester notre site avant de le déployer.

Il se lance à l'aide de la commande suivante :

```bash
hugo server --disableFastRender --noHTTPCache
```

`--disableFastRender` permet d'avoir la dernière version sur le serveur  
`--noHTTPCache` permet de ne rien avoir de sauvegardé dans le cache du navigateur

Il reste possible que certains détails ne soient pas à jours sur le serveur de test. Si les modifications ne sont pas présentes, même après avoir réfraichi la page, arretez le serveur en faisant `Ctrl + C` puis relancez-le avec la commande ci-dessus.

---

## Déployer le site

Pour déployer votre site web, vosu devez uniquement prendre le dossier `public` qui contient tous les fichiers finaux.

Vous pouvez ensuite le déployer selon la méthode de votre choix sur le support de votre choix.

---

## Plus d'informations

Pour plus d'information sur la configuration ou sur la personnalisation du site, consultez la documentation offcelle de Blowfish :

<https://blowfish.page/>

---
