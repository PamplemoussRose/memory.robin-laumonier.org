---
title: "Routes web des pages"
summary: "Fonctionnement des routes pour acceder aux differentes pages du site"
date: "2026-07-29"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Les routes

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

{{< alert >}}
**Attention !** La route `/XXX/{id}` doit imperativement être la dernière dans l'ordre ! Sinon toutes les routes `/XXX/YYY` palcées après ne seront jamais ateintes car aspirées par `{id}` !
{{< /alert >}}

---

## Passer des données à une vue

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
