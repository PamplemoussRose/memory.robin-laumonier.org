---
title: "Personnalisation du site"
summary: "Méthodes pour personnaliser les composant est les styles utilisé sur le site"
date: "2026-07-02"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Personnalisation du site

Pour presonnaliser facilement notre site, nous pouvons utiliser deux modules nous facilitant la tâche :

- Tailwind va nous aider pour le style des composants avec des classes précréées contenant déjà les paramêtres CSS voulu
- DaisyUI va nous aider pour la construction des composants avec des éléments existant sur leurs site

Pour les deux modules, nosu allons utiliser le CDN pour les environements de developpement. Les CDN sont des scripts qui vont analyser la page et importer les informations nécessaires pour faire le rendu final. Ce processus n'est pas utilisé en production car cela rend le chargement des pages beaucoup plus lourd et peut provoquer des flash des composant le temps que les classes soient importées. Dans un environement de production, nous allons installer les modules directement dans le projet pour avoir un site plus réactif lors du chargement des pages.

---

## Installation de Tailwind

Voir toute la documentation et les classes Tailwind à l'adresse suivante :  
[**https://tailwindcss.com/docs**](https://tailwindcss.com/docs)

### Tailwind pour le developpement

Pour inclure le CDN Tailwind à notre projet, nous devons ajouter la ligne suivante entre les balises `<head>` des pages :

```html
<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
```

Ensuite, nous pouvons utiliser les classes Tailwind normalement dans notre code HTML.

### Tailwind pour la production





---

## Installation de DaisyUI

Voir toute la documentation et les composants DaisyUI à l'adresse suivante :  
[**https://daisyui.com/docs**](https://daisyui.com/docs)

### DaisyUI pour le developpement

Pour inclure le CDN DaisyUI à notre projet, nous devons ajouter les lignes suivantes entre les balises `<head>` des pages :

```php
// Ajouter DaisyUI
<link href="https://cdn.jsdelivr.net/npm/daisyui@5" rel="stylesheet" type="text/css" />
<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
// Ajouter tous les thèmes mis à disposition par DaisyUI
<link href="https://cdn.jsdelivr.net/npm/daisyui@5/themes.css" rel="stylesheet" type="text/css" />
```

{{< alert >}}
**Attention !** Si vous prévoyez d'utiliser un thème personnalisé, n'ajoutez pas les thèmes DaisyUI et passez à la section [**Créer des thèmes DaisyUI**](#créer-des-thèmes-daisyui) !
{{< /alert >}}

Les styles DaisyUI sont construits avec les classes Tailwind. Nous retrouvons donc le CDN Tailwind dans les choses à inclure.

Ensuite, nous pouvons utiliser les classes Tailwind normalement dans notre code HTML.

### DaisyUI pour la production





### Créer des thèmes DaisyUI

DaisyUI propose de personnaliser les thèmes existant ou d'en créer un nouveau pour utiliser nos propres couleurs.

Il est possible de choisir les couleurs du thème en passant par l'UI du site de DaisyUI :  
[**https://daisyui.com/theme-generator**](https://daisyui.com/theme-generator)

Ajouter un thème personnalisé est différent si vous êtes en dev ou en prod.

En dev, allez dans votre fichier CSS et ajoutez les lignes suivantes en adaptant le nom de votre thème:

```css
:root:has(input.theme-controller[value=mytheme]:checked),[data-theme="mytheme"] {
    /* mytheme est le nom du thème personnalisé */
    color-scheme: light;
    --color-base-100: oklch(98% 0.02 240);
    /* ... reste des couleurs choisies pour le thème */
}
```

Utiliser DaisyUI ne nous empêche pas d'utiliser les classes Tailwind pour affiner la personnalisation de nos composants !

---
