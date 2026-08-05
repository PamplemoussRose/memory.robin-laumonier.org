---
title: "Authentification"
summary: "Fonctionnement de l'authentification et des sessions"
date: "2026-07-24"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Composants utilisateurs déjà présents

Quand vous créez une application, Laravel et Herd créent directement une migration pour la table en base de données et un model pour la gestion des utilisateurs.

C'est composant sont situé dans `database/migrations/XXX_XX_XX_XXXXX_create_users_table.php` et `app/Models/User.php`.

## Creation d'utilisateur

Pour la création d'un utilisateur, nous devons créer un nouveau controller basique, par exemple `RegisteredUserController.php` :

```php
php artisan make:controller auth/RegisteredUserController
```

En ajoutant `/auth` avant le nom, Laravel va automatiquement créer un nouveau dossier (s'il n'existe pas) et placer le nouveau controller dans le dossier `app/http/controller/auth/`

Ce controller va être composé de deux méthodes :

- `create` qui va charger la page de création d'utilisateur
- `store` qui va ajouter le nouvel utilisateur en base de données et le connecter sur le site

Elles auront la forme suivante :

```php
public function create() {
    return view('auth.register');
}

public function store(Request $request) {

    // validate request
    $request->validate([
        'name' => ['required', 'string', 'max:255'],
        'email' => ['required', 'string', 'email', 'max:255', 'unique:users'],
        'password' => ['required', 'string', 'min:12'],
    ]);

    // create user in DB
    $user = User::create([
        'name' => $request->name,
        'email' => $request->email,
        'password' => Hash::make($request->password),
    ]);

    // log in user
    Auth::login($user);

    // redirect to home page
    return redirect('/');
    }
```

Il faut maintenant créer la vue, les routes utilisant le controller et relier un formulaire d'inscription à l'endpoint `/register`

`auth.register.blade.php` :

```php
<x-layout-page title="Ajouter un utilisateur">
    <form action="/register" method="POST">
        @csrf
        <fieldset>
            // Formulaire
        </fieldset>
    </form>
</x-layout-page>
```

Routes :

```php
// Charge la page pour ajouter un utilisateur
Route::get('/register', [RegisteredUserController::class, 'create']);
// Ajouter un utilisateur
Route::post('/register', [RegisteredUserController::class, 'store']);
```

Quand l'utilisateur va envoyer le formulaire, le controller va verifier que tous les champs sont bien remplis et que les données sont au bon format puis, si tout est correcte, connecter l'utilisateur.

---
