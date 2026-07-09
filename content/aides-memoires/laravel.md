---
title: "Site web en utilisant Laravel"
summary: "Tutoriel pour créer un site web en se basant sur le framework Laravel"
date: "2026-07-07"
categories: ["tuto"]
tags: ["site web", "php", "laravel"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Instalation de l'environement de développement

### Développement

Pour pouvoir développer en utilisant le framework Laravel, vous devez avoir un environement comprenant :

- `php`
- `composer`
- `laravel`
- `expose`
- `node`
- `npm`
- `nvm`

Vous pouvez soit les installer manuellement, soit utiliser [**Laravel Herd**](https://herd.laravel.com/) qui est un utilitaire qui va se charger de tout installer pour vous.  
La version gratuite est suffisante pour créer un site.

### Base de données

Par défaut, Herd utilise SQLite comme base de données. Si vous souhaitez utiliser un autre driver, veillez à l'installer pour la suite.

---

## Creation d'une application Laravel

Pour la suite du tutoriel, nous allons assumer que :

- L'installation a été réalisée via Herd
- Le driver de base de données utilisé est MySQL
- Aucun *starter kit* n'est utilsé
- Les pages seront des templates *Blade*

### Creation de l'application

Pour créer une application Laravel, rendez-vous dans le dossier Herd qui est par défaut `C:\Users\user\Herd` puis éxecutez la commande suivante :

```sh
laravel new nom_app
```

Ensuite, suivez la configuration dans le terminal pour finaliser la création.  
Vous pouvez également utiliser un des *Starter Kits* disponibles nativement avec Laravel.

### Modification de la base de donées

Pour changer la base de données utilisée par l'applciation, vous devez modifier le fichier `.env` qui se trouve dans le dossier `~\Herd\nom_app`.

Les variables à changer sont :

- `DB_CONNECTION`, le driver de base de données utiliser (ex : mysql, sqlite...)
- `DB_HOST`, l'adresse pour contacter la base de données
- `DB_PORT`, le port utilisé par la base de données
- `DB_DATABASE`, le nom de la base de données
- `DB_USERNAME`, le nom de l'utilisateur que va utiliser l'application
- `DB_PASSWORD`, le mort de passe de l'utilisateur que va utiliser l'application

### Accès à l'application

Une fois la configuration terminée, vous pouvez accéder à l'application via l'URL `http://nom_app.test` (uniquement si l'instalation a été réalisée avec Herd).

---

## Fonctionnement de Laravel

### Les routes

Les routes vers les differentes pages du site sont gérées depuis le fichier `web.php` qui est dans le dossier `routes`.

Chaque route est par un bloc du type :

```php
Route::get('/about', function () {
    return view('about');
});
```

Ce bloc fait en sorte de charger la vue (page) `about` lorsque l'utilisateur accède à l'URL `nom_app.test/about`.

Si la route va uniquement charger une page, le code peut être réduit et écrit de la manière suivante :

```php
Route::view('/about', 'about');
```

Pour ajouter des routes au site, il suffit de dupliquer le bloc et de l'adapter aux nouvelles pages. Au final, le fichier `web.php` aura la forme suivante :

```php
Route::get('/', function () {
    return view('welcome');
});

Route::get('/products', function () {
    return view('products');
});

Route::get('/contact', function () {
    return view('contact');
});

Route::get('/about', function () {
    return view('about');
});
```

### Les vues

Pour chaque route ajoutée au fichier `web.php`, il faut créer la vue correspondante.  
Une vue est un ficher php qui contient le code de la page à afficher. Elles sont situées dans le dossier `resources/views`.

### Les styles et scripts

Pour modifier les styles des classes que vous utilisés sur votre site, allez dans le dossier `public` à la racine de votre projet et créez votre fichier `.css` (ex : `app.css`).

Le principe est le même si bous voulez ajouter des scripts javascript. Dans le dossier `public`, créez un fichier `.js` (ex : `app.js`).

Pour inclure ces deux fichiers dans les pages, ajoutez les lignes suivantes dans la balise HTML `head` :

```html
<link rel="stylesheet" href="/app.css">
<script src="/app.js"></script>
```

### Les composants

Les composants sont des fichiers contenants du code HTML qui vont pouvoir être réutilisés partout sur le site. Ils  peuvent conerner le header, le footer ou tout autre éléments que vous souhaitez utiliser plusieurs fosi à tarvers votre site.

Ces fichiers sont stocké dans le dossier `resources/views/components`. Si le dossier n'existe pas, vous pouvez le créer et y ajouter vos composants.

Pour utiliser un composant `component.blade.php`, allez sur la vue devant l'utiliser, appelez le avec les balises `<x-component></x-component>` puis joutez ensuite le code que vous souhaitez entre celles-ci.

#### Les variables

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

#### Layout

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

### Passer des données à une vue

Lors du chargement d'une vue, il est possible de lui donner des données à utiliser.

Il y a deu xmanières de faire :

1. Récupérer les données en interne

    ```php
    Route::view('/', 'welcome', [
        'data' => 'data_to_use'
    ]);

    Route::get('/', function () {
        return view('welcome', [
            'data' => 'data_to_use'
        ]);
    });
    ```

    Lors du chargement de la vue, la balise `{{$data}}` dans le composant va etre remplacé par `data_to_use`.

2. Récupérer les données de l'url

    ```php
    Route::view('/', 'welcome', [
        'data'=>request('data_url', 'data_default')
    ]);

    Route::get('/', function () {
        return view('welcome', [
            'data'=>request('data_url', 'data_default') 
        ]);
    });
    ```

    Dans le cas de l'URL, le composant va regarder si un paramêtre `data_url` est présent et introduir la valeur à la place de la balise `{{$data}}` du composant. Si il n'y a pas de paramêtre `data_url`, le composant va prendre la valeur par défaut qui a été renseigné, ici `data_default`.

    Par défaut, les templates blade s'occupent de gérer les failles de sécurités liées aux injection de code dans l'url. Cependant, il est possible de desctiver ces protections et de faire completement confiance à l'utilisateur en utilisant la balise `{{!!$data!!}}` dans le code du composant.

---

### Directives Blade

Blade remplace en grande majorité les mécanismes PHP par des directives. Ces directives sont appelée via le symbole `@` suivit du mot clé.

Voici une liste non-hexaustive des directives les plus courantes :

#### IF / ELSE

```php
@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don't have any records!
@endif
```

#### FOR

```php
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor
```

#### FOREACH

```php
@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach
```

#### WHILE

```php
@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

#### SWITCH

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
- `@auth(role)/@endauth`            : Regarde si l'utilisateur est connecté au rôle `role`
- `@guest/@endguest`                : Regarde si l'utilisateur n'est pas connecté
- `@guest(role)/@endguest`          : Regarde si l'utilisateur n'est pas connecté au rôle `role`
- `@can(action, variable)/@endcan`  : Vérifie si l'utisateur à les droits pour faire `action` sur `variable`

---
