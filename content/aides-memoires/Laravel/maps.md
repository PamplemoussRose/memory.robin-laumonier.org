---
title: "Intégration de cartes"
summary: "Tuto pour intégrer des cartes dans les pages du site"
date: "2026-07-01"
categories: ["tuto"]
tags: ["site web", "framework", "php", "laravel"]
layout: "page"
showBreadcrumbs: true
draft: "false" # Set to true if this page is not to be shown
---

---

Ce tuto montre la méthode pour intégrer des cartes en utilisant l'API **Google Maps** et **Leaflet**.  
La partie code général est à inclure peu importe la solution choisie.

## Code général

Créer une table pour contenir les points dans la base de données :

```sh
php artisan make:migration create_points_table
```

Dans le fichier `database\migrations\XXX_create_points_table.php`, ajouter les champs pour contenenir le nom et les coordonnéees des points :

```php
Schema::create('points', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->decimal('lat', 10, 7);
    $table->decimal('lng', 10, 7);
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
use Illuminate\Http\JsonResponse;

class PointController extends Controller
{
    public function index(): JsonResponse
    {
        return response()->json(
            Point::select('id', 'name', 'lat', 'lng')->get()
        );
    }

    public function map()
    {
        $points = Point::select('id', 'name', 'lat', 'lng')->get();

        return view('map', compact('points'));
    }
}
```

Ajouter la route pour charger la page dans `routes/api.php` :

```php
use App\Http\Controllers\PointController;

Route::get('/carte', [PointController::class, 'map'])->name('map');
```

Ajouter les routes API au projet avec la commande `php artisan install:api` puis ajouter une route dans `routes/api.php` pour recupérer les points :

```php
use App\Http\Controllers\PointController;

Route::get('/points', [PointController::class, 'index']);
```

Dans le fichier de layout (`layout-page.blade.php`), ajouter deux emplacements pour les styles et les scripts :

```php
<!DOCTYPE html>
<html>
<head>
    ...
    {{ $styles ?? '' }}
</head>
<body>
    {{ $slot }}

    {{ $scripts ?? '' }}
</body>
</html>
```

`{{ $styles ?? '' }}` doit être placé dans le `<head>`, `{{ $scripts ?? '' }}` juste avant `</body>`. 

Vérifier que les fichiers pour les marqueurs et clusters sont bien présents dans le dossier d'image :

- `public/img/marina-marker.png`
- `public/img/cluster.png`

---

## Leaflet

Pour leaflet, ajoutez le code suivant dans la vue qui doit contenir la carte :

```php
<x-layout-page title="Carte">

    <x-slot:styles>
        <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
        <link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css" />
        <link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css" />
    </x-slot:styles>

    <div id="map" style="width:100%;height:600px;"></div>

    <x-slot:scripts>
        <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
        <script src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"></script>
        <script>
          const markerData = @json($points);

          const map = L.map('map').setView([46.5, 2.5], 6);

          L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; OpenStreetMap contributors',
          }).addTo(map);

          const markerIcon = L.icon({
            iconUrl: '/img/marina-marker.png',
            iconSize: [32, 32],
            iconAnchor: [16, 32],
          });

          const clusterGroup = L.markerClusterGroup({
            iconCreateFunction: function (cluster) {
              return L.divIcon({
                html: `<div style="position:relative;width:48px;height:48px;">
                         <img src="/img/cluster.png" style="width:48px;height:48px;" />
                         <span style="position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);color:#fff;font-size:12px;font-weight:bold;">
                           ${cluster.getChildCount()}
                         </span>
                       </div>`,
                className: '',
                iconSize: [48, 48],
              });
            },
          });

          markerData.forEach((point) => {
            const marker = L.marker([point.lat, point.lng], { icon: markerIcon });
            marker.bindPopup(point.name);
            clusterGroup.addLayer(marker);
          });

          map.addLayer(clusterGroup);
        </script>
    </x-slot:scripts>

</x-layout-page>
```

Les paramêtres comme le centre, le zoom ou les couleurs par defaut de la carte sont à configurer selon votre besoin.

---

## Google Maps

Pour Google Maps, il faut d'abord enregistrer la clé API pour utiliser l'API Google :

Ajoutez la clé avec les variables d'environelent dans le fichier `.env` :

```txt
GOOGLE_MAPS_API_KEY=xxxxxxxxxxxxxxxxxxxxxxxx
```

Ajoutez l'API Google aux services utilisés par l'application dans le fichier `config/services.php` :

```php
'google_maps' => [
    'key' => env('GOOGLE_MAPS_API_KEY'),
],
```

Ensuite, ajoutez le code suivant dans la vue qui doit contenir la carte :

```php
<x-layout-page>

    <div id="map" style="width:100%;height:600px;"></div>

    <x-slot:scripts>
        <script>
          window.markerData = @json($points);
        </script>
        <script>
          function initMap() {
            const map = new google.maps.Map(document.getElementById("map"), {
              zoom: 6,
              center: { lat: 46.5, lng: 2.5 },
            });

            const markers = window.markerData.map((point) => {
              const marker = new google.maps.Marker({
                position: { lat: point.lat, lng: point.lng },
                icon: {
                  url: "/img/marina-marker.png",
                  scaledSize: new google.maps.Size(32, 32),
                },
                title: point.name,
              });

              const infowindow = new google.maps.InfoWindow({
                content: point.name,
              });
              marker.addListener("click", () => infowindow.open(map, marker));

              return marker;
            });

            new markerClusterer.MarkerClusterer({
              map,
              markers,
              renderer: {
                render: ({ count, position }) =>
                  new google.maps.Marker({
                    position,
                    icon: {
                      url: "/img/cluster.png",
                      scaledSize: new google.maps.Size(48, 48),
                    },
                    label: {
                      text: String(count),
                      color: "#ffffff",
                      fontSize: "12px",
                    },
                  }),
              },
            });
          }
        </script>
        <script src="https://unpkg.com/@googlemaps/markerclusterer/dist/index.min.js"></script>
        <script
          src="https://maps.googleapis.com/maps/api/js?key={{ config('services.google_maps.key') }}&callback=initMap"
          async
          defer
        ></script>
    </x-slot:scripts>

</x-layout-page>
```

Les paramêtres comme le centre, le zoom ou les couleurs par defaut de la carte sont à configurer selon votre besoin.

---
