---
title: "Requests"
summary: "Fonctionnement et interractions des composants Request"
date: "2026-07-26"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
layout: "page"
showBreadcrumbs: true
draft: "false" # Set to true if this page is not to be shown
---

---

## Définition

Une **Request** représente une requête HTTP envoyée à l'application Laravel. Elle contient toutes les données transmises par le client :

- paramètres de l'URL
- données d'un formulaire
- corps d'une requête JSON
- fichiers envoyés
- en-têtes HTTP (headers)
- informations sur l'utilisateur authentifié

Laravel fournit deux principaux types de Requests :

- **Request** : accès aux données de la requête.
- **Form Request** : validation et autorisation centralisées.

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
Request
   │
   ├── Lecture des données
   ├── Validation
   ├── Autorisation
   │
   ▼
Controller
   │
   ▼
Model
   │
   ▼
Réponse
```

---

## Le Request classique

Le Request classique est une instance de :

```php
Illuminate\Http\Request
```

Il est injecté automatiquement dans les méthodes du controller.

Exemple :

```php
use Illuminate\Http\Request;

class UserController extends Controller
{
    public function store(Request $request)
    {
        //
    }
}
```

---

## Récupérer des données

### Tous les champs

```php
$request->all();
```

### Un champ

```php
$request->input('name');
```

ou

```php
$request->name;
```

### Avec une valeur par défaut

```php
$request->input('role', 'user');
```

### Plusieurs champs

```php
$request->only([
    'name',
    'email'
]);
```

### Tous sauf certains champs

```php
$request->except([
    'password'
]);
```

### Vérifier la présence d'un champ

```php
$request->has('email');
```

### Vérifier qu'un champ est rempli

```php
$request->filled('email');
```

### Vérifier qu'un champ est absent

```php
$request->missing('email');
```

---

## Les fichiers

Vérifier la présence :

```php
$request->hasFile('avatar');
```

Récupérer le fichier :

```php
$file = $request->file('avatar');
```

Enregistrer le fichier :

```php
$path = $request->file('avatar')->store('avatars');
```

---

## Les données JSON

Laravel lit automatiquement le JSON.

Exemple :

```json
{
    "name": "Alice",
    "email": "alice@test.com"
}
```

Accès :

```php
$request->input('name');
```

Aucune différence avec un formulaire classique.

---

## Les paramètres de route

Route :

```php
Route::get('/users/{id}', ...);
```

Accès :

```php
$id = $request->route('id');
```

---

## Les informations utilisateur

Utilisateur connecté :

```php
$request->user();
```

Vérifier l'authentification :

```php
$request->user() !== null
```

---

## Les en-têtes HTTP

Lire un header :

```php
$request->header('Authorization');
```

Tous les headers :

```php
$request->headers->all();
```

---

## Validation rapide

Il est possible de valider directement dans un controller.

```php
$request->validate([
    'name' => 'required',
    'email' => 'required|email'
]);
```

Cette méthode est adaptée aux petits projets ou à des validations simples.

---

## Les Form Requests

Une **Form Request** est une classe dédiée à :

- l'autorisation
- la validation
- la préparation des données

Elle permet d'alléger les controllers.

---

## Création

Commande :

```bash
php artisan make:request StoreUserRequest
```

Le fichier est créé dans :

```txt
app/
└── Http/
    └── Requests/
        └── StoreUserRequest.php
```

---

## Structure

```php
class StoreUserRequest extends FormRequest
{
    public function authorize(): bool
    {
        return true;
    }

    public function rules(): array
    {
        return [
            //
        ];
    }
}
```

### authorize()

Détermine si l'utilisateur peut effectuer l'action.

Exemple :

```php
public function authorize(): bool
{
    return auth()->check();
}
```

Ou :

```php
return $this->user()->isAdmin();
```

Si la méthode retourne :

```php
false
```

Laravel renvoie automatiquement une réponse **403 Forbidden**.

### rules()

Contient toutes les règles de validation.

Exemple :

```php
public function rules(): array
{
    return [
        'name' => 'required|max:255',
        'email' => 'required|email|unique:users',
        'age' => 'nullable|integer|min:18'
    ];
}
```

---

## Utilisation dans un controller

Au lieu d'utiliser :

```php
Request $request
```

on injecte :

```php
StoreUserRequest $request
```

Exemple :

```php
public function store(StoreUserRequest $request)
{
    //
}
```

La validation est exécutée automatiquement avant l'appel de la méthode.

---

## Les données validées

Toutes les données validées :

```php
$request->validated();
```

Un champ :

```php
$request->validated('email');
```

Ou :

```php
$request->safe()->only([
    'name',
    'email'
]);
```

---

## Messages d'erreur personnalisés

Méthode :

```php
public function messages(): array
{
    return [
        'name.required' => 'Le nom est obligatoire.',
        'email.email' => 'Adresse email invalide.'
    ];
}
```

---

## Attributs personnalisés

Permet de remplacer les noms techniques des champs.

```php
public function attributes(): array
{
    return [
        'name' => 'nom',
        'email' => 'adresse électronique'
    ];
}
```

Message obtenu :

```txt
Le champ nom est obligatoire.
```

---

## Préparer les données

Méthode :

```php
protected function prepareForValidation(): void
{
    $this->merge([
        'email' => strtolower($this->email)
    ]);
}
```

Exécutée avant la validation.

---

## Ajouter une validation après les règles

```php
public function after(): array
{
    return [
        function ($validator) {
            if ($this->age < 18) {
                $validator->errors()->add(
                    'age',
                    'Vous devez être majeur.'
                );
            }
        }
    ];
}
```

Pratique pour des validations complexes.

---

## Règles de validation courantes

| Règle | Description |
| -------- | ------------- |
| required | Champ obligatoire |
| nullable | Champ facultatif |
| string | Chaîne de caractères |
| integer | Nombre entier |
| numeric | Nombre |
| boolean | Booléen |
| array | Tableau |
| email | Adresse email valide |
| url | URL valide |
| date | Date valide |
| min:x | Valeur minimale |
| max:x | Valeur maximale |
| between:x,y | Valeur comprise entre x et y |
| size:x | Taille exacte |
| confirmed | Vérifie le champ `_confirmation` |
| unique:table | Valeur unique |
| exists:table,column | Valeur existante en base |
| in:a,b,c | Valeur parmi une liste |
| regex:... | Expression régulière |
| file | Fichier |
| image | Image |
| mimes:jpg,png,pdf | Extensions autorisées |
| max:2048 | Taille maximale (Ko pour les fichiers) |

---

## Validation conditionnelle

Exemple :

```php
public function rules(): array
{
    return [
        'company' => [
            'required_if:type,professional'
        ]
    ];
}
```

---

## Validation avec des objets Rule

```php
use Illuminate\Validation\Rule;

public function rules(): array
{
    return [
        'status' => [
            Rule::in([
                'draft',
                'published',
                'archived'
            ])
        ]
    ];
}
```

---

## Bonnes pratiques

- Utiliser une **Form Request** par formulaire ou endpoint.
- Laisser la logique métier dans des Services ou des Models, pas dans la Request.
- Utiliser `validated()` plutôt que `all()` pour éviter de traiter des données non validées.
- Centraliser les règles de validation dans les Form Requests.
- Utiliser `prepareForValidation()` pour normaliser les données avant validation.
- Réserver `authorize()` au contrôle d'accès lié à la requête.

---

## Comparatif

| Caractéristique | Request | Form Request |
| ----------------- | --------- | -------------- |
| Accès aux données | Oui | Oui |
| Validation | Manuelle | Automatique |
| Autorisation | Non | Oui (`authorize()`) |
| Messages personnalisés | Limités | Oui |
| Préparation des données | Non | Oui |
| Réutilisable | Faiblement | Oui |
| Controller allégé | Non | Oui |

---

## Les commandes Artisan

Créer une Form Request :

```bash
php artisan make:request StoreUserRequest
```

---

## Résumé

| Besoin | Solution recommandée |
| --------- | ---------------------- |
| Lire les données d'une requête | `Request` |
| Valider quelques champs rapidement | `$request->validate()` |
| Validation complète et réutilisable | `FormRequest` |
| Contrôler les permissions d'une action | `authorize()` |
| Normaliser les données avant validation | `prepareForValidation()` |
| Personnaliser les messages d'erreur | `messages()` |
| Récupérer uniquement les données validées | `validated()` |

---
