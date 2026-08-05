---
title: "Construction d'un formulaire"
summary: "Fonctionnement et construction de formulaire pour le site"
date: "2026-07-25"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Construction

La construction des formulaire n'ai rien de different comparé à du html classique. On retrouve la même balise `<script></script>`, le concept de champs et de bouton de validation.

---

## Sécurité

Laravel integre par défaut une protection contre les CSRF (Cross-Site Request Forgery) qui se fait en intégrant un token au formulaire. Lors de la validation du formulaire, le serveur va vérifier si le token est compatible avec celui du serveur. En cas de token non valide, une erreur va être remontée et le formulaire ne va pas être traité. Si le token est valide, les données vont être traitée normalement.

Pour inclure le token de vérification, il suffit d'ajouter la directive `@csfr` au fomulaire.

---

## Validation du formulaire

Pour que le formulaire soit traité, nous devons ajouter une route vers l'adresse et avec la méthode utilisée par le formuilaire.

Cela se fait comme l'ajout de route classiques dans le fichier `web.php` :

```php
Route::post('/form', function () {
    // code à ajouter
    return redirect('/form'); // Redirection apres le traitement des données
});
```

Avant de récupérer les données soumises, nous allons vérifer que le formulaire envoie bien les données.

Pour ce faire, nous allons utiliser la methode `dd` qui permet de dump ce qui lui est passé en parametre sur la page.

```php
Route::post('/form', function () {
    dd("Sanity check");
});
```

Si tout fonctionne correctement le resultat est :

```php
"Sanity check" // routes\web.php:37
```

---

## Vérification du format des donénes

Une fois que le formulaire est correctement redirigé, nous devons vérifier que les données envoyées sont bien dans dans le format attendu.

Nous allons donc créer une classe de type *request* pour notre formuaire. Chaque classe *request* est associée à un formulaire car elle va contenir les règles de validation des données et la liste des utilisateurs authorisé à faire les modifcations.

La classe est crée avec la commande suivante :

```php
php artisan make:request
```

Ce type de classe comporte deux méthodes :

- `authorize()` qui renvoie un booléen déterminant si l'utilisateur peut faire l'action du formulaire. Si la vérification échoue, le serveur renvoie une erreur 403 unauthorized.
- `rules()` qui contient la liste des règles de validation pour les données entrantes du formulaire.

Il est également possible d'ajouter une méthode `message()` afin de personnaliser les messages d'erreur renvoyé quand une entrée ne suit pas les règles :

```php
public function messages(): array
    {
        return [
            'name.required' => 'Veuillez renseigner le nom',
            'name.min' => 'Le nom doit avoir au moins 10 caracteres',
            // Il est possible d'appeler le nom du champ en utilisant `:attribute`
        ];
    }
```

Comme cette classe étend la classe `Request`, il est possible de l'appeler à la place de cette dernière dans les controllers de ressources :

```php
    public function store(StorePostRequest $request)
    {
        Post::create([
            'name' => request('name')
            // ...
        ])
    }
```

De cette façon, nous allons être sûr que les données entrantes sont bien dans le format voulu

---

## Récupération des données du formulaire

Pour récupérer les données, il y a plusieurs methodes utilisables :

```php
Route::post('/form', function () {
    // Récupère tous les champs
    request()=>all()
    // Récupère le champs 'field'
    $field = request('field')
    // ajouter `use Illuminate\Support\Facades\Request;` au debut du fichiers
    $field = Request::input('field')
});
```

Ou encore :

```php
// ajouter `use Illuminate\Http\Request;` au debut du fichier
Route::post('/form', function (Request $request) {
    $request->input('field');
    $request->field;
});
```

---
