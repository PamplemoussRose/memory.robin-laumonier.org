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

## Connexion et Deconnexion

Pour gérer les sessions des utilisteurs, nous allons créer un nouveau controller avec les méthodes `create`, `store` et `destroy`. Ces méthodes correspondent au chargement du formulaire de connexion, à la tentative de connexion ainsi qu'à la deconnexion d'un utilisateur.

```php
class SessionsController extends Controller
{
    public function create()
    {
        return view('auth.login');
    }
    
    public function store(Request $request)
    {
        // validate data
        $validated = $request->validate([
            'email' => ['required', 'string', 'email', 'max:255'],
            'password' => ['required', 'string', Password::default()],
        ]);

        // log in user
        if(Auth::attempt($validated)){
            $request->session()->regenerate();

            return redirect('/');
            // return redirect()->intended('/');
        }

        // redirect
        return back()->withErrors([
            'email' => 'Le courriel ou le mot de passe est incorrect.',
        ]);
    }
    
    public function destroy()
    {
        Auth::logout();

        return redirect('/');
    }
}
```

Lors de la redirection après la connexion, la méthode `->intended()` redirige vers la page que l'utilisateur essaiyait d'atteindre sans être connecté. Si l'utilisateur vse connectait simplement, il va être redirigé vers la route par défaut aui est entre parenthèses.

Les routes de connexions auront le format suivant :

```php
// Affiche le formulaire de connexion
Route::get('/login', [SessionsController::class, 'create']);
// Connecte un utilisateur
Route::post('/login', [SessionsController::class, 'store']);
// Déconnecte un utilisateur
Route::delete('/logout', [SessionsController::class, 'destroy']);
```

Et la vue de connexion sera un formulaire comme suit :

```php
<x-layout-page title="Ajouter un utilisateur">
    <form action="/login" method="POST">
        @csrf
        <fieldset class="fieldset bg-base-200 border-base-300 rounded-box w-xs border m-4 p-4 mx-auto">
            <legend class="fieldset-legend">Se connecter</legend>

            <label class="label" for="email">Courriel</label>
            <input type="email" class="input" name="email" placeholder="Votre Couriel" required/>

            <label class="label">Mot de passe</label>
            <input type="password" class="input" name="password" placeholder="Votre mot de passe" required/>
            <x-form.error nom="password"/>
            <x-form.error nom="email"/>
            <button class="btn btn-neutral mt-4">Se connecter</button>
        </fieldset>
    </form>
</x-layout-page>
```

---

##

---
