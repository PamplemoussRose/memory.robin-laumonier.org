---
title: "Controllers"
summary: "Fonctionnement et interractions des controllers"
date: "2026-07-27"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
layout: "page"
showBreadcrumbs: true
draft: "false" # Set to true if this page is not to be shown
---

## Définition

Un **Controller** est une classe responsable de traiter les requêtes HTTP. Il agit comme intermédiaire entre :

- les **routes**
- les **modèles (Models)**
- les **vues (Views)** ou les réponses JSON

Son objectif est de séparer la logique métier de la définition des routes afin de rendre le projet plus organisé et maintenable.

---

## Fonctionnement général

Flux d'une requête :

```txt
Client
   │
   ▼
Route
   │
   ▼
Controller
   │
   ├── Appel des Models
   ├── Validation
   ├── Traitement métier
   └── Préparation de la réponse
   │
   ▼
Vue HTML ou JSON
```

Exemple :

```php
Route::get('/users', [UserController::class, 'index']);
```

```php
class UserController extends Controller
{
    public function index()
    {
        return User::all();
    }
}
```

---

## Création d'un controller

Créer un controller simple :

```bash
php artisan make:controller UserController
```

Le fichier est créé dans :

```txt
app/
└── Http/
    └── Controllers/
        └── UserController.php
```

---

## Les différents types de Controllers

Laravel permet de générer plusieurs types de controllers adaptés à différents besoins.

---

### 1. Controller classique

Commande :

```bash
php artisan make:controller UserController
```

Structure :

```php
class UserController extends Controller
{
    public function index()
    {

    }

    public function show($id)
    {

    }
}
```

#### Utilisation

- plusieurs méthodes
- logique variée
- plusieurs routes

Exemple :

```php
Route::get('/users', [UserController::class, 'index']);
Route::get('/users/{id}', [UserController::class, 'show']);
```

---

### 2. Resource Controller

Commande :

```bash
php artisan make:controller UserController --resource
```

Laravel génère automatiquement les 7 méthodes CRUD.

```php
class UserController extends Controller
{
    public function index() {}

    public function create() {}

    public function store(Request $request) {}

    public function show(User $user) {}

    public function edit(User $user) {}

    public function update(Request $request, User $user) {}

    public function destroy(User $user) {}
}
```

Route associée :

```php
Route::resource('users', UserController::class);
```

Cette seule ligne crée toutes les routes CRUD.

| Méthode | URL | Action |
| --------- | ------ | -------- |
| GET | /users | index |
| GET | /users/create | create |
| POST | /users | store |
| GET | /users/{user} | show |
| GET | /users/{user}/edit | edit |
| PUT/PATCH | /users/{user} | update |
| DELETE | /users/{user} | destroy |

**Utilisation :**

Applications web classiques utilisant des vues.

---

### 3. API Resource Controller

Commande :

```bash
php artisan make:controller Api/UserController --api
```

ou

```bash
php artisan make:controller UserController --api
```

Différence :

Les méthodes **create()** et **edit()** sont supprimées car une API ne retourne pas de formulaires HTML.

Méthodes générées :

```php
index()
store()
show()
update()
destroy()
```

Route :

```php
Route::apiResource('users', UserController::class);
```

Routes créées :

| Méthode | URL |
| --------- | ----- |
| GET | /users |
| POST | /users |
| GET | /users/{user} |
| PUT/PATCH | /users/{user} |
| DELETE | /users/{user} |

**Utilisation :**

API REST.

---

### 4. Invokable Controller

Un Invokable Controller possède une seule méthode :

```php
__invoke()
```

Commande :

```bash
php artisan make:controller ProfileController --invokable
```

Résultat :

```php
class ProfileController extends Controller
{
    public function __invoke()
    {
        //
    }
}
```

Route :

```php
Route::get('/profile', ProfileController::class);
```

Aucune méthode n'est précisée.

**Utilisation :**

Une seule action :

- accueil
- tableau de bord
- page contact
- génération de PDF
- export Excel

#### Avantages

- très simple
- une responsabilité unique
- facile à tester

---

### 5. Singleton Resource Controller

Introduit pour représenter une ressource unique appartenant à un contexte, sans identifiant dans l'URL.

Commande :

```bash
php artisan make:controller ProfileController --resource --singleton
```

Exemple :

Un utilisateur possède **un seul profil**.

Mauvaise URL :

```txt
/profiles/15
```

Bonne URL :

```txt
/profile
```

Routes :

```php
Route::singleton('profile', ProfileController::class);
```

Routes générées :

| Méthode | URL | Action |
| --------- | ------ | -------- |
| GET | /profile | show |
| GET | /profile/edit | edit |
| PUT/PATCH | /profile | update |

Il n'y a pas d'identifiant dans l'URL.

#### Cas d'utilisation

- profil utilisateur
- paramètres
- panier courant
- abonnement
- tableau de bord personnel

---

### 6. API Singleton Controller

Version API du Singleton.

Commande :

```bash
php artisan make:controller ProfileController --singleton --api
```

Routes :

```php
Route::apiSingleton('profile', ProfileController::class);
```

Routes générées :

| Méthode | URL |
| --------- | ----- |
| GET | /profile |
| PUT/PATCH | /profile |

Selon les options (`--creatable` ou `--destroyable`), Laravel peut également générer :

- POST
- DELETE

**Utilisation :**

API où une seule ressource existe.

Exemples :

```txt
/me
/settings
/profile
/preferences
```

---

### Comparatif

| Type | Plusieurs actions | CRUD | HTML | API | Identifiant |
| ------- | ------------------- | ------ | ------ | ----- | ------------- |
| Standard | Oui | Libre | Oui | Oui | Oui |
| Resource | Oui | Oui | Oui | Possible | Oui |
| API Resource | Oui | Oui | Non | Oui | Oui |
| Invokable | Non | Non | Oui | Oui | Selon le besoin |
| Singleton | Oui | Oui | Oui | Non | Non |
| API Singleton | Oui | Oui | Non | Oui | Non |

---

## Les options Artisan

Créer un controller classique :

```bash
php artisan make:controller UserController
```

Controller Resource :

```bash
php artisan make:controller UserController --resource
```

Controller API :

```bash
php artisan make:controller UserController --api
```

Controller Invokable :

```bash
php artisan make:controller UserController --invokable
```

Controller Singleton :

```bash
php artisan make:controller UserController --resource --singleton
```

Controller API Singleton :

```bash
php artisan make:controller UserController --singleton --api
```

---

## Bonnes pratiques

- Un controller doit avoir une responsabilité claire.
- Éviter d'y placer une logique métier complexe ; utiliser des Services ou des Actions dédiées.
- Valider les données avec des **Form Requests** plutôt que directement dans le controller.
- Exploiter l'**Injection de dépendances** pour les services.
- Utiliser le **Route Model Binding** afin d'obtenir automatiquement les modèles.

Exemple :

```php
public function show(User $user)
{
    return view('users.show', compact('user'));
}
```

Laravel récupère automatiquement l'utilisateur correspondant à l'identifiant présent dans l'URL.

---

## Résumé

| Besoin | Controller recommandé |
| --------- | ------------------------ |
| Plusieurs routes personnalisées | Controller classique |
| CRUD complet avec vues | Resource Controller |
| API REST | API Resource Controller |
| Une seule action | Invokable Controller |
| Ressource unique (profil, paramètres...) | Singleton Resource Controller |
| Ressource unique dans une API | API Singleton Controller |

---
