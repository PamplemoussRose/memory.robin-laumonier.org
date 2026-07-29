---
title: "Constructions des vues"
summary: "Fonctionnement et construction des vue pour les pages du site "
date: "2026-07-30"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Les vues

Pour chaque route ajoutée au fichier `web.php`, il faut créer la vue correspondante.  
Une vue est un ficher php qui contient le code de la page à afficher. Elles sont situées dans le dossier `resources/views`.

---

## Les styles et scripts

Pour modifier les styles des classes que vous utilisés sur votre site, allez dans le dossier `public` à la racine de votre projet et créez votre fichier `.css` (ex : `app.css`).

Le principe est le même si bous voulez ajouter des scripts javascript. Dans le dossier `public`, créez un fichier `.js` (ex : `app.js`).

Pour inclure ces deux fichiers dans les pages, ajoutez les lignes suivantes dans la balise HTML `head` :

```html
<link rel="stylesheet" href="/app.css">
<script src="/app.js"></script>
```

---

## Les composants

Les composants sont des fichiers contenants du code HTML qui vont pouvoir être réutilisés partout sur le site. Ils  peuvent conerner le header, le footer ou tout autre éléments que vous souhaitez utiliser plusieurs fosi à tarvers votre site.

Ces fichiers sont stocké dans le dossier `resources/views/components`. Si le dossier n'existe pas, vous pouvez le créer et y ajouter vos composants.

Pour utiliser un composant `component.blade.php`, allez sur la vue devant l'utiliser, appelez le avec les balises `<x-component></x-component>` puis joutez ensuite le code que vous souhaitez entre celles-ci.

---

### Les variables

Chaque composant doit contenir une variable `{{$slot}}` qui sera l'endroit ou sera situé ce qui sera ajouté lors de l'utilisation.

Vous pouvez ajouter d'autres variables pour avoir des éléments plus adaptables aux pages. Il faudra donc spécifier au compoosant quelle valeur doit prendre la variable lors de l'appel. Par exemple, si un composant `composant.blade.php` utiliser une variable `{{$varaible}}`, l'appel du composant va être :

```php
<x-component variable="valeur">
<!--- code html ajouté --->
</x-component>
```

Il est possible de forcer une variable à avoir une valeur pour utiliser le composant.

Pour ce faire, il faut ajouter la directive `@props` au debut du composant :

```php
@props([
    'variable1',
    'variable2'
])
```

Il est également possible de spécifier une valeur par défaut :

```php
@props([
    'variable'=>'valeur_par_defaut',
    'variable2'=>'valeur_par_defaut2'
])
```

Toute variable qui n'est pas dans la liste des props sera considérée comme un attribut. Il sera donc possible de les manipuler selon les besoins.

Par exemple, dans le cas des classes de balise HTML, il est possible de definir une classe qui sera tout le temps utliser et de faire en sorte de pouvoir en passer d'autres en attribut lors de l'appel du composant pour les ajouter à l'endroit précis de cet appel.

Dans le code du composant, nous allons regarder si des classes ont été passées en attribut. Si oui, les nouvelles classes seront ajoutées à celle de base; Si non, seule la classe de base sera utilisée.

Le code du composant sera comme suivant :

```php
<div {{ $attributes->merge(['class'=>'class1']) }}>
    {{ $slot }}
</div>
```

Si l'attribut `class` est présent lors de l'appel du composant, les classes seront ajoutées à `class1`.

L'utilisation du composant avec les classes à ajouter se fera comme suivant :

```php
<x-composant class="class2">
    <!--- code html ajouté --->
</x-composant>
```

Le résultat sera l'ajout des classes passées en attribut au classes actives sur le bloc :

```html
<div class"class1 class2">
    <!--- code html ajouté --->
</div>
```

---

### Layout

Le composant *layout* est généralement celui utilisé pour la forme globale des pages. Si vous voulez avoir plusieurs dispositions à travers votre site, vous pouvez créer plusieurs *layout* et les utliser selon les pages.

Un layout classique peut avoir la forme suivante :

```php
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Titre</title>
</head>
<body>
<main>
    {{$slot}}
</main>
</body>
</html>
```

Ou avec des variables :

```php
@props([
    'title'=>'nom_app'
])

<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>{{$title}}</title>
</head>
<body>
<main>
    {{$slot}}
</main>
</body>
</html>
```

---

### Styles et Scripts unique à une page

Il est également possible d'ajouter des emplacements pour du code css ou javascript qui doit être ajouter sur une page unique.

Pour ce faire nous allons placer des variables qui seront vide par défaut dans le layout et les assoscier à du code lors de la construction de la page si besoins.

Code du fichier `layout.blade.php` :

```php
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>{{$title}}</title>
    <link rel="stylesheet" href="/app.css">
    {{ $styles ?? '' }}
    <script src="/app.js"></script>
</head>
<body>
<main>
    {{$slot}}
    {{ $scripts ?? '' }}
</main>
</body>
</html>
```

Code de la page utilisant le layout :

```php
<x-layout title="Titre">

    // Code de la page

    <x-slot:styles>
        // CSS pris en compte uniquement sur cette page
    </x-slot:styles>

    <x-slot:scripts>
        // Javascript pris en compte uniquement sur cette page
    </x-slot:scripts>
</x-layout>
```

---

### Composants généraux

En dehors du layout, il est possible de créer autant de composant que voulu. Il suffit de les appeller selon leurs nom comme le composant layout avec les balises `<x-nom-composant></x-nom-composant>`.

Vous pouvez aussi les organiser dans des dossier. Pour appeller des composant dans des dossier, tapez le chemin pour accéder au composant après le `x-` dans la balise :

```php
<x-dossier.nom-composant>
    // Code inséré à la place de la variable $slot si présente
</x-dossier.nom-composant>
```

---

## Directives Blade

Blade remplace en grande majorité les mécanismes PHP par des directives. Ces directives sont appelée via le symbole `@` suivit du mot clé.

Voici une liste non-hexaustive des directives les plus courantes :

### IF / ELSE

```php
@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don't have any records!
@endif
```

### FOR

```php
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor
```

### FOREACH

```php
@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach
```

### WHILE

```php
@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

### SWITCH

```php
@switch($i)
    @case(1)
        First case...
        @break

    @case(2)
        Second case...
        @break

    @default
        Default case...
@endswitch
```

### UNLESS

`@unless` est une directive qui est équivalente à `@if(! condition)` :

```php
@unless (Auth::check())
    You are not signed in.
@endunless
```

### FORELSE

`@forelse` est une directive qui permet de gérer des cas spécifiques du contenant à parcourir.

```php
@forelse ($users as $user)      // Parcours de la liste
    <li>{{ $user->name }}</li>  // Action pour chaque element de la liste
@empty                          // Détéction du cas où la liste est vide
    <p>No users</p>             // Action dans le cas d'une liste vide
@endforelse                     // Fin de la boucle
```

### Control de droit

Il y a également des directives pour vérifier le status et les permissions de l'utilisateur avant de faire ce qui est dans la directive :

- `@auth/@endauth`                  : Regarde si l'utilisateur est connecté
- `@auth(role)/@endauth`            : Regarde si l'utilisateur est connecté avec rôle `role`
- `@guest/@endguest`                : Regarde si l'utilisateur n'est pas connecté
- `@guest(role)/@endguest`          : Regarde si l'utilisateur n'est pas connecté avec rôle `role`
- `@can(action, variable)/@endcan`  : Vérifie si l'utisateur à les droits pour faire `action` sur `variable`

---

## Rooting pour les ressources

Pour facilité la lecture de l'architecture des dossiers, il est possible de regrouper les pages concernant le même sujet :

```txt
Racine du projet :
└───resources
    └───views
        │   welcome.blade.php
        │   about.blade.php
        ├───components
        │       layout.blade.php
        │       header.blade.php
        │       footer.blade.php
        └───post
                index.blade.php
                show.blade.php
                edit.blade.php
```

Les pages dans le dossier `resource/view` sont des pages classques, celles dans le dossier `resources/view/components` sont des templates utilisables pour construir les autres pages et celles dans `resources/view/post` sont des pages qui sont en rapport avec les postes.

Les pages dans le dernier cas devront être référencée via `post.nom_page` pour lier la vue à une adresse.

---