---
title: "Site web en utilisant Laravel"
summary: "Tutoriel pour créer un site web en se basant sur le framework Laravel"
date: "2026-07-07"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
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

Vous pouvez soit les installer manuellement, soit utiliser Laravel Herd ([**https://herd.laravel.com/**](https://herd.laravel.com/)) qui est un utilitaire qui va se charger de tout installer pour vous.  
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

{{< alert >}}
**Attention !** La route `/XXX/{id}` doit imperativement être la dernière dans l'ordre ! Sinon toutes les routes `/XXX/YYY` palcées après ne seront jamais ateintes car aspirées par `{id}` !
{{< /alert >}}

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

#### UNLESS

`@unless` est une directive qui est équivalente à `@if(! condition)` :

```php
@unless (Auth::check())
    You are not signed in.
@endunless
```

#### FORELSE

`@forelse` est une directive qui permet de gérer des cas spécifiques du contenant à parcourir.

```php
@forelse ($users as $user)      // Parcours de la liste
    <li>{{ $user->name }}</li>  // Action pour chaque element de la liste
@empty                          // Détéction du cas où la liste est vide
    <p>No users</p>             // Action dans le cas d'une liste vide
@endforelse                     // Fin de la boucle
```

#### Control de droit

Il y a également des directives pour vérifier le status et les permissions de l'utilisateur avant de faire ce qui est dans la directive :

- `@auth/@endauth`                  : Regarde si l'utilisateur est connecté
- `@auth(role)/@endauth`            : Regarde si l'utilisateur est connecté au rôle `role`
- `@guest/@endguest`                : Regarde si l'utilisateur n'est pas connecté
- `@guest(role)/@endguest`          : Regarde si l'utilisateur n'est pas connecté au rôle `role`
- `@can(action, variable)/@endcan`  : Vérifie si l'utisateur à les droits pour faire `action` sur `variable`

---

### Formulaires

#### Construction

La construction des formulaire n'ai rien de different comparé à du html classique. On retrouve la même balise `<script></script>`, le concept de champs et de bouton de validation.

#### Sécurité

Laravel integre par défaut une protection contre les CSRF (Cross-Site Request Forgery) qui se fait en intégrant un token au formulaire. Lors de la validation du formulaire, le serveur va vérifier si le token est compatible avec celui du serveur. En cas de token non valide, une erreur va être remontée et le formulaire ne va pas être traité. Si le token est valide, les données vont être traitée normalement.

Pour inclure le token de vérification, il suffit d'ajouter la directive `@csfr` au fomulaire.

#### Validation du formulaire

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

#### Récupération des données du formulaire

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

### Bases de données

#### Création des tables

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

{{< alert >}}
**Attention !** Si vous voulez utilisez Eloquent, mettez le nom de la base de données au pluriel car lors de la requetes, Eloquent prend le nom du model et cherche une base de données avec ce nom au pluriel (Si le model est `Point.php`, Eloquent va chercher une table nommée `points` dans la base de données).
{{< /alert >}}

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

#### Récupération des données

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

### Base de données avec Eloquent

Eloquent permet pas la création de nouvelles bases de données. Cependant, il nous facilite la gestion de ces bases avec la création de model representant les lignes de données.

#### Models

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

#### SELECT

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

#### INSERT

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

#### UPDATE

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

#### DELETE

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

#### Gestion des erreurs

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

### Rooting pour les ressources

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

### Leaflet

Créer une table pour contenir les points dans la base de données :

```sh
php artisan make:migration create_points_table
```

Dans le fichier `database\migrations\XXX_create_points_table.php`, ajouter les champs pour contenenir le nom et les coordonnéees des points :

```php
Schema::create('points', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->double('lat');
    $table->double('lng');
    $table->timestamps();
});
```

Créer le fichier `app/model/Point.php` et y ajouter le code suivant :

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Point extends Model
{
    protected $fillable = ['name', 'lat', 'lng'];

    protected $casts = [
        'lat' => 'float',
        'lng' => 'float',
    ];
}
```

Créer un controleur pour les points dans le fichier `app/Http/Controllers/PointController.php` :

```php
<?php

namespace App\Http\Controllers;

use App\Models\Point;

class PointController extends Controller
{
    public function index()
    {
        return Point::select('id', 'name', 'lat', 'lng')->get();
    }
}
```

Ajouter les routes API au projet avec la commande `php artisan install:api` puis ajouter une route pour recupérer les points :

```php
use App\Http\Controllers\PointController;

Route::get('/points', [PointController::class, 'index']);
```

Dans la page qui doit contenir la page, ajoutez le code suivant :

```php
<!--- Div contenant la carte --->
<div id="map" class="w-full h-[600px]"></div>

<!--- Scripts pour peupler et le fonctionnement de la carte --->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
    document.addEventListener('DOMContentLoaded', function () {
        const map = L.map('map').setView([46.6, 2.5], 6);

        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; OpenStreetMap contributors'
        }).addTo(map);

        fetch('/api/points')
            .then(res => res.json())
            .then(points => {
                points.forEach(p => {
                    L.marker([p.lat, p.lng])
                        .addTo(map)
                        .bindPopup(p.name);
                });
            });
    });
</script>
```

---
