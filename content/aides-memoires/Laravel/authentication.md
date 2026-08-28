---
title: "Authentification"
summary: "Fonctionnement de l'authentification et des sessions"
date: "2026-07-24"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
layout: "page"
showBreadcrumbs: true
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

Lors de la redirection après la connexion, utiliser `return redirect()->intended()` redirige vers la page que l'utilisateur essaiyait d'atteindre sans être connecté. Si l'utilisateur se connectait simplement, il va être redirigé vers la route par défaut aui est entre parenthèses.

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

## Accès aux pages demmendant une connexion

Certaines pages de l'application ne vont pouvoir être accessible uniquement si l'utilisateur est connecté.

Pour gérer ce cas de figure, Laravel porpose d'utiliser un `middleware` qui va nous permetre de rediriger les requêtes vers la page de connexion :

```php
Route::get('/post', [PostsController::class, 'index'])->middleware('auth');
```

Si vous avez plusieurs pages qui ne sont pas accessibles sans connexion, vous pouvez regrouper les routes de la façon suivante :

```php
Route::middleware('auth')->group(function () {

    // Toutes les routes accessibles uniquement si utilisateur connectés

    // ex :
    Route::get('/post', [PostController::class, 'index']);
    Route::get('/post/create', [PostController::class, 'create']);
    // ...

});
```

Il est possible de faire la même chose avec `guest` pour les pages accessibles uniquement en tant qu'invité :

```php
Route::middleware('guest')->group(function () {
    // Toutes les routes accessibles uniquement si utilisateur non connectés
});
```

Avec l'utilisation de la méthode `auth`, le middleware va chercher une URL assossiée au nom de route "login" et rediriger l'utilisateur.

Pour assossier une URL, il y a deux méhodes :

- Ajouter la l'URL dans le fichier `bootstrap\app.php` :

```php
return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        api: __DIR__.'/../routes/api.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    ->withMiddleware(function (Middleware $middleware): void {
        $middleware->redirectGuestsTo('/login');
        $middleware->redirectGuestsTo('/');
    })
    ->withExceptions(function (Exceptions $exceptions): void {
        $exceptions->shouldRenderJsonWhen(
            fn (Request $request) => $request->is('api/*'),
        );
    })->create();
```

- Assossier la route vers la page de connexion au nom "login" dans le fichier `routes\web.php`

```php
Route::get('/login', [SessionsController::class, 'create'])->name('login');
```

---

## Filter sur l'utilisateur actuel

Pour que chaque utilisateur ne vois que ses propres éléments, nous allons chercher en base de données uniquement les lignes qui le concernent.

Pour ce faire, nous allons changer la requête vers la base de données :

```php
// Requête globale
$posts = Post::all()
// Requête pour récupérer uniquement les lignes de l'utilisateur
$posts = Post::query()->where([
    'user_id' => Auth::id(),
])->get();
    // OU si vous avew définit des relations entre vos models Eloquents
$posts = Auth::user()->posts;
```

---

## Accéès aux pages selon un rôle

Si vous avez des pages qui sont accessibles selon des rôles utilisateur, vous pouvez définir des règles appelées *Gates* dans le fichier `app/Providers/AppServiceProvider.php`.

Ces gates sont plcées dans la méthodes `boot` et renvoient des booléens après avoir vérifier les autorisations de l'utilisateur :

```php
    public function boot(): void
    {
        Gate::define('view-admin', function (User $user) {
            return $user->isAdmin();
        });

        // Autres gates...
    }
```

Il est possible de customiser le code de renvoie de la gate avec la classe `Illuminate\Auth\Access\Response` qui propose :

- `allow()`
- `deny()`
- `denyWithStatus()`
- `denyAsNotFound()`

Ces gates peuvent être utilisées pour protéger des éléments dans les pages ou des endpoints du site.

Pour les éléments d'une page, la protection se fait via la directive `@can()` avec le nom de la gate :

```php
@can('view-admin')
    <a href="/admin">Admin<a>
@endcan
```

Pour la securisation des endpoints du site, vous pouvez le faire soit par groupe dans `web.php`, soit par route dans le controller utilisé par la route.

Dans `web.php` :

```php
Route::can('view-admin')->group(function () {

    // Routes à protéger

}

// OU

Route::get('/posts', [PostController::class, 'index'])->can('view-admin');
```

Dans le controller :

```php

public function index()
    {
        Gate::authorize('view-admin');
        // Reste de la méthode
    }
```

---

## Règles sur les models

Laravel porpose d'appliquer des règles aux model en utilisant des `policy`. Vous pouvez les créer et les lier aux models avec la commande suivante :

```sh
php artisan make:policy PostPolicy --model=Post
```

Cela va créer le dossier `app/policies` s'il n'existe pas et y placer les fichiers `policy`.

Lorsau'il est généré, le fichier policy contient plusieurs méthodes que vous pouvez renommer ou compléter selon les rèlges que vous voulez mettre :

```php
/**
 * Determine whether the user can view the model.
 */
public function view(User $user, Post $post): Response
{
    // Logique de contrôle
    return $user->id === $point->user_id ? Response::allow() : Response::denyAsNotFound();
}
```

Pour l'utilistion des policies dans le code, nous allons faire appel aux gates avec le nom de la policy et l'objet sur lequel l'appliquer.

Par exemple, dans un controller :

```php
    public function show(Post $post)
    {
        Gate::authorize('update', $post);
        return view('post.show', ['post' => $post]);
    }
```

---
