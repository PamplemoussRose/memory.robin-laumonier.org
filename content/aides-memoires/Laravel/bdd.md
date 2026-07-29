---
title: "Base de données"
summary: "Fonctionnement et construction de la base de données pour le site"
date: "2026-07-27"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

## Création des tables

Pour créer les tables de notre base de données, nous allons utiliser les migrations. Chaque ficher migration est lié à une table et décrit sa structure.

La création d'un fichier migration se fait via la méthode `make:migration` dans un terminal :

```sh
php artisan make:migration nom_migration
```

Vous pouvez alors choisir le nom de votre fichier (généralement en lien avec la table qu'il va créer) puis retourner dans votre éditeur.

Le nouveau fichier va se trouver dans le dossier `database/migrations` depuis la racine de votre projet et sera nommé selon le model `[timestamp]_[nom_choisi].php`.

Un fichier migration suit le model suivant :

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('users_info', function (Blueprint $table) {
            $table->id();
            // Autres champs SQL
            $table->timestamp('created_at')->useCurrent();
            $table->timestamp('updated_at')->useCurrent()->useCurrentOnUpdate();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('users_info');
    }
};
```

La méthode `up` sert à créer et mettre à jour les tables tandis que la méthode `down` sert à annuler les modifications faites par `up`.

Il est possible de gérer plusieurs tables dans le même fichier migration :

```php
public function up(): void
    {
        Schema::create('users', function (Blueprint $table) {
            // Champs SQL
        });

        Schema::create('password_reset_tokens', function (Blueprint $table) {
            // Champs SQL
        });

        Schema::create('sessions', function (Blueprint $table) {
            // Champs SQL
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('users');
        Schema::dropIfExists('password_reset_tokens');
        Schema::dropIfExists('sessions');
    }
```

Lors de la création des champs dans un fichier migration, il est possible d'ajouter des methodes correspondant à des attributs SQL :

| Méthode | corresspondance SQL |
| --- | --- |
| `->primary()` | `PRIMARY_KEY` |
| `->autoIncrement()` | `AUTO_INCREMENT` |
| `->comment('my comment')` | Ajoute un commentaire à la colonne |
| `->nullable()` | Accepte `NULL` comme valeur |
| `->unsigned()` | `UNSIGNED` |
| `->useCurrent()` | Les colonnes `TIMESTAMP` vont utiliser `CURRENT_TIMESTAMP` par defaut |
| `->useCurrentOnUpdate()` | Les colonnes `TIMESTAMP` vont utiliser `CURRENT_TIMESTAMP` s'il y a une misa à jour |
| `->default()` | Fixe la valeur par defaut |

Pour la gestion des clés étrangères, une manière de faire est de définir une colonne puis de lier la clé primaire d'une autre table :

```php
Schema::create('posts', function (Blueprint $table) {
    // Création de la colonne dans la nouvelle table
    $table->unsignedBigInteger('user_id');

    $table->foreign('user_id')  // Colonne locale
        ->references('id')      // Colonne distante
        ->on('users')           // Table distante
        ->onUpdate('cascade')   // Propagation des mises à jours
        ->onDelete('cascade');  // Propagation des suppressionss
});
```

Une fois le fichier migration terminé, il faut appliquer les changements à la base de données avec la commande :

```sh
php artisan migrate
```

La commande `migrate` contient des arguments pour effectuer des actions plus spécifiques :

```txt
migrate:fresh             Drop all tables and re-run all migrations
migrate:install           Create the migration repository
migrate:refresh           Reset and re-run all migrations
migrate:reset             Rollback all database migrations
migrate:rollback          Rollback the last database migration
migrate:status            Show the status of each migration
```

---

## Récupération des données

Laravel transforme automatiquement les collection en json quand retourné :

```php
Route::get('/forms', function () {
    $users_info = DB::table('users_info')->get(); // Import `use Illuminate\Support\Facades\DB`;
    dd($users_info);
});
```

```json
Illuminate\Support\Collection {#295 ▼ // routes\web.php:40
  #items: array:2 [▼
    0 => {#301 ▼
      +"id": 1
      +"username": "user1"
      +"about": "desc1"
      +"created_at": "2026-07-13 11:44:37"
      +"updated_at": "2026-07-13 11:44:37"
    }
    1 => {#300 ▼
      +"id": 2
      +"username": "user2"
      +"about": "desc2"
      +"created_at": "2026-07-13 11:45:37"
      +"updated_at": "2026-07-13 11:45:37"
    }
  ]
  #escapeWhenCastingToString: false
}
```

```php
Route::get('/forms', function () {
    $users_info = DB::table('users_info')->get(); // Import `use Illuminate\Support\Facades\DB`;
    return $users_info;
});
```

```json
[
  {
    "id": 1,
    "username": "user1",
    "about": "desc1",
    "created_at": "2026-07-13 11:44:37",
    "updated_at": "2026-07-13 11:44:37"
  },
  {
    "id": 2,
    "username": "user2",
    "about": "desc2",
    "created_at": "2026-07-13 11:45:37",
    "updated_at": "2026-07-13 11:45:37"
  }
]
```

Pour avoir une ligne seulement, il faut intéragir avec la collection comme un avec un array classique :

```php
return $users_info[0];
```

```json
{
  "id": 1,
  "username": "user1",
  "about": "desc1",
  "created_at": "2026-07-13 11:44:37",
  "updated_at": "2026-07-13 11:44:37"
}
```

Et pour avoir un champs spécifique d'une ligne, il faut aller chercher la variable en question :

```php
return $users_info[0]->username;
```

```txt
user1
```

Il est posible d'utiliser des methodes comme `get()` ou `where('colonne','valeur')` sur `DB::table('nom_table')` mais il est aussi possible décrire la requete SQL complète en utilisant `DB::select('requete_SQL')`.

Pour plus de détails consultez la documentation Laravel :  
[**https://laravel.com/docs/13.x/queries**](https://laravel.com/docs/13.x/queries).

---

## Base de données avec Eloquent

Eloquent permet pas la création de nouvelles bases de données. Cependant, il nous facilite la gestion de ces bases avec la création de model representant les lignes de données.

### Models

Les models Eloquent sont créé via la commande suivante :

```php
php artisan make:model
```

Dans le cas d'un model pour la base de données, un model simple suffit.

Tous les models créés sont trouvables dans le dossier `app\Models` et ont par defaut la structure suivante :

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    //
}
```

Vous pouvez spécifier la table à laquelle est relié le model avec la ligne suivante :

```php
protected $table = 'nom_table';
```

Les attributs des models Eloquent peuvent être mis dans deux catégories :

- `$guarded` : catégiorie par défaut des attibuts, elle bloque l'assignation de masse
- `$fillable` : autorise l'assignation de masse

Il est possible de gérer la classification des attibuts avec la structure suivante :

```php
class Post extends Model
{
    protected $guarded = [
        // attibuts à bloquer
    ];
    protected $fillable = [
        // attributs à liberer
    ];
}
```

### Controllers

Pour utiliser facilement les models, il est recommander de passer par des controllers.

Les controllers sont des objets qui vont contenir les methodes utiles à la manipulation de la ressource assossiée.

```php
namespace App\Http\Controllers;

use App\Models\Post;
use Illuminate\Http\Request;

class PostController extends Controller
{
    /**
     * Display a listing of the resource.
     */
    public function index()
    {
        //
    }
    /**
     * Store a newly created resource in storage.
     */
    public function store(Request $request)
    {
        //
    }
    /**
     * Update the specified resource in storage.
     */
    public function update(Request $request, Haltes_cyclable $halte_cyclable)
    {
        //
    }

    // Les autres méthodes sont à la suite
}
```

Pour être complet, un controller doit avoir les méhtodes `index`, `create`, `store`, `show`, `edit`, `update` et `destroy` qui servent respectivement à récupérer toutes les données de la table, affichier la page pour ajouter une linge, ajouter une ligne dans la base de données, récupérer les informations d'une ligne précise, affichier la page de modification d'une ligne, modifier une ligne dans la base de données et supprimer une ligne dans la base de données.

Une fois les méthodes crées, il est possible des les appeller dans les routes du site en reliant un chemin à une des méthodes du controller.

Par exemple, pour afficher tous les posts, nous allons lier la méthode `index` au chemin `/post` de la manière suivante :

```php
Route::get('/post', [PostController::class, 'index']);
```

### SELECT

Pour selectionner des lignes dans la base de donnée avec Eloquent, nous utilisons le model qui vient d'être créer :

```php
// Pour avoir tous les posts
$posts = Post::all()
// Pour filtrer sur une colonne
$posts = Post::where('title', 'title_exemple')->get();
```

Très imporant d'utiliser la méthode `get()` si vous filter pour garantir le bon format de retour de données.

Il est également possible de filtrer selon des paramètres passés dans l'URL.

Si la requête est la suivante `test-app.test/posts?state=pending`, nous pouvons récupérer la valeur du paramètre `state` et l'inclure dans une requête vers la base de données :

```php
Route::get('/post', function() {
    $posts = Post::query()
    ->when(request('state'), function($query, $state) {
        $query->where('state', $name);
    })->get();
})
```

### INSERT

Pour récupérer les données d'un formulaire, les deux méthodes suivantes donnent le même résultat :

```php
Route::post('/post', function () {
    $username = request('username');
    $about = request('about');
    Post::create([
        'username' => $username,
        'about' => $about,
    ]);
    return redirect('/post');
});

Route::post('/post', function () {
    Post::create([
        'username' => request('username'),
        'about' => request('about'),
    ]);
    return redirect('/post');
});
```

Lors de la création d'objet en base de données avec Eloquent, les timestamps sont mis à jour automatiquement.

### UPDATE

Pour la mise a jour des informations, la méthode HTTP utilisée est `PATCH`. Cependant, les navigateurs n'envoient que des requètes `GET` ou `POST`. Pour régler ce problème, il suffit d'ajouter la directive `method` dans le formulaire pour indiquer à Laravel quelle méthode exatement doit être utilisée :

```php
<form method="POST" action="/post/{{$post->id}}">
    @csrf
    @method('PATCH')
    <!--- Reste du formulaire --->
</form>
```

Une fois que le fiormulaire est prêt, il faut créer une nouvelle route qui va faire l'action de mettre à jour les données :

```php
Route::patch('/post/{user}', function(Post $post) {
    $post->update([
        'username' => request('username'),
        'about' => request('about'),
    ]);
    return redirect("post/{$user->id}");
});
```

On utilise ici une route avec la méthode PATCH car c'est celle qui a été paramètrée dans le formulaire.

### DELETE

Pour supprimer une ligne de la base de données, nous allons utiliser le même principe que pour la mise à jour.

Pour commencer, nous devons avoir un formulaire qui pointe vers l'endpoint de suppression avec la directive `@method('DELETE')` :

```php
<!--- Bouton pour supprmier --->
<button form="delete-user-form" type="submit">
    Delete
</button>
<!--- Form à valider pour activer l'endpoint --->
<form id="delete-user-form" method="POST" action="/point/{{$point->id}}">
    @csrf
    @method('DELETE')
</form>
```

Ensuite nous devons associer la route au fait de supprimer la ligne de l'id donné :

```php
Route::delete('/point/{point}', function(Point $point) {
    $point->delete();
    return redirect('/point');
});
```

Il est également possible de vide completement la table en utilisant la méthode `Point::truncate()`.

### Gestion des erreurs

Dans certains cas, une erreur peut survenir suite à la requête en base de données.

Par exemple l'utilisation de `find($id)` renvoie `null` si aucun id ne correspond à la recheche ce qui peut entrainer des erreurs de lecture sur null lors du chargement de la page.

Pour éviter ce type de scénario, il est possible de faire de la gestion d'erreur à la suite de la requête pour rediriger vers la page 404 :

```php
Route::get('/post/{id}', function ($id) {
    $post = Post::find($id);
    if (is_null($post)) {
        abort(404);
    }
    // OU
    $post = Post::findOrFail($id);

    return view('post.show', [
        "post" => $post,
    ]);
});
```

Dans les deux cas, l'utilisateur va arriver sur la page 404 du site si l'id passé dans l'URL n'est pas dans la table des postes.

Il est possible de réduire encore plus le nombre de ligne en liant la route et le modèle :

```php
Route::get('/users-info/{user}', function(Users_info $user) {
    return view('users-info.show', [
        "title" => "User information",
        "user_info" => $user,
    ]);
});
```

Avec cette structure, Laravel va chercher une ligne avec un id égale à la valeur de `{user}` dans la table assossiée au modele `Users_info`. Il est obligatoire que le nom de la variable entre accoloade et celle après le $ soit le même car Laravel se base sur ces information pour faire le lien entre ce que retourne la base de données et ce qui est utilisable dans le code.

---
