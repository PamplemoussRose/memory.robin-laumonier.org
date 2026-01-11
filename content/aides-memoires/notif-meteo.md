---
title: "Notifications météo quotidiennes"
summary: "Mise en place d'un système pour envoyer des notifications quotidiennes avec la météo du jour sur un IPhone."
date: "2025-12-18"
categories: ["tuto"]
tags: ["python", "notification", "iphone", "météo"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Architecture du Projet

Le projet repose sur l'architecture suivante :

| Composant | Rôle | Solution Technique |
| :--- | :--- | :--- |
| **Ordonnanceur** | Déclenche l'exécution du script à une heure fixe. | **Cron** (sur Raspberry Pi) |
| **Moteur** | Script qui récupère, traite les données et envoie la notification. | **Python** avec la librairie `requests` |
| **Source Météo** | Fournit les données de prévisions. | **Open-Meteo** (API gratuite, sans clé) |
| **Service de Notification** | Transmet le message push à l'iPhone. | **ntfy.sh** (service push simple avec application iOS native) |
| **Client** | Reçoit et affiche la notification. | **Application ntfy** (sur votre iPhone) |

---

## Configuration de l'iPhone

Pour recevoir les notifications, vous devez configurer l'application `ntfy` sur votre iPhone.

1. **Installation :** Téléchargez et installez l'application `ntfy` depuis l'App Store.
2. **Abonnement à un topic :** Dans l'application, vous devez vous abonner à un topic qui servira de canal de communication. Ce topic doit être secret pour éviter que d'autres personnes n'envoient des notifications sur votre téléphone.
Pour ce faire, appuyez sur le boutton + dans l'application, choisissez le nom du topic et appuyez sur `subscribe` pour valider sa création.
3. **Test du topic :** Vous pouvez testez le topic avec la commande suivante :

```sh
curl -d "hi" ntfy.sh/NOM_DU_TOPIC
```

Vous devrez alors recevoir une notifaction sur votre téléphone avec le message `hi` dans le corps de celle-ci.

---

## Préparation du Raspberry Pi

Nous allons maintenant configurer le Raspberry Pi.

### Installation des Dépendances

Le script utilise Python et la librairie `requests` pour les requêtes HTTP. Vous pouvez les installer avec les commandes suivantes.

```sh
# 1. Mise à jour du système
sudo apt update && sudo apt upgrade -y

# 2. Installation de Python et de pip (si non déjà fait)
sudo apt install python3 python3-pip -y

# 3. Installation de la librairie requests
pip3 install requests
```

### Création du Script Python

Le script `meteo_notifier.py` est responsable de la logique.

1. **Créez le fichier :**

    ```sh
    nano meteo_notifier.py
    ```

2. **Collez le contenu du script :**

    ```python
    import requests
    import json
    from datetime import datetime, timedelta

    # --- Configuration ---
    # Nom du topic ntfy.sh
    NTFY_TOPIC = "meteo_notifier_polycoloc"
    # Coordonnées de Montréal (utilisées comme exemple)
    LATITUDE = 45.5017
    LONGITUDE = -73.5673
    # URL de l'API Open-Meteo
    WEATHER_API_URL = "https://api.open-meteo.com/v1/forecast"
    # URL du serveur ntfy.sh
    NTFY_SERVER_URL = f"https://ntfy.sh/{NTFY_TOPIC}"

    def get_weather_forecast() -> dict:
        """Récupère les prévisions météo pour la journée en cours."""
        params = {
            "latitude": LATITUDE,
            "longitude": LONGITUDE,
            "daily": "weather_code,temperature_2m_max,temperature_2m_min,snowfall_sum,rain_sum,wind_speed_10m_max", # Données à récupérer
            "timezone": "America/Montreal", # Timezone de votre machine
            "forecast_days": 1 # On ne veut que les données d'aujourd'hui
        }

        try:
            response = requests.get(WEATHER_API_URL, params=params)
            response.raise_for_status() # Lève une exception pour les codes d'erreur HTTP
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f"Erreur lors de la récupération des données météo : {e}")
            return None

    def extract_daily_summary(data) -> tuple[str, str, int]:
        """Extrait les prévisions de la journée."""
        if not data or 'daily' not in data:
            return "Données météo indisponibles."

        daily_data = data['daily']
        weather_code = daily_data['weather_code'][0]
        temparatures_max = daily_data['temperature_2m_max'][0]
        temperatures_min = daily_data['temperature_2m_min'][0]
        snowfall = daily_data['snowfall_sum'][0]
        rain = daily_data['rain_sum'][0]
        wind_speed = daily_data['wind_speed_10m_max'][0]

        # Fonction simple pour interpréter le code météo (WMO Weather interpretation codes)
        def interpret_weather_code(code) -> str:
            codes = {
                0: "Ciel dégagé",
                1: "Principalement dégagé",
                2: "Partiellement nuageux",
                3: "Couvert",
                45: "Brouillard",
                48: "Brouillard givrant",
                51: "Bruine légère",
                53: "Bruine modérée",
                55: "Bruine dense",
                61: "Pluie légère",
                63: "Pluie modérée",
                65: "Forte pluie",
                71: "Neige légère",
                73: "Neige modérée",
                75: "Forte neige",
                80: "Averses légères",
                81: "Averses modérées",
                82: "Averses violentes",
                95: "Orage",
                96: "Orage avec grêle légère",
                99: "Orage avec forte grêle"
            }
            return codes.get(code, "Météo inconnue")

        # Construction du message
        message_title = f"Meteo Montreal le {datetime.now().strftime('%d/%m')}"

        if snowfall = 0 and rain = 0 :
            message_body = (
            f"{interpret_weather_code(weather_code)}\n"
            f"Entre {temperatures_min}°C et {temparatures_max}°C et vent à {wind_speed} km/h."
            )
        if snowfall > 0 and rain = 0 :
            message_body = (
            f"{interpret_weather_code(weather_code)} avec {snowfall} cm de neige. "
            f"Entre {temperatures_min}°C et {temparatures_max}°C et vent à {wind_speed} km/h."
            )
        elif snowfall = 0 and rain > 0 :
            message_body = (
            f"{interpret_weather_code(weather_code)} avec {rain} mm de pluie. "
            f"Entre {temperatures_min}°C et {temparatures_max}°C et vent à {wind_speed} km/h."
            )
        else :
            message_body = (
            f"{interpret_weather_code(weather_code)} avec {rain} mm de pluie et {snowfall} cm de neige. "
            f"Entre {temperatures_min}°C et {temparatures_max}°C et vent à {wind_speed} km/h."
            )

        return message_title, message_body, weather_code

    def send_ntfy_notification(title, message, weather_code) -> bool:
        """Envoie la notification via ntfy.sh."""
        headers = {
            "Title": title,
            "Priority": "default", # 'default', 'high', 'urgent'
            "Content-Type": "text/plain"
        }
        match weather_code:
            case 0 :
                headers["Tags"] = "sunny"
            case 1 :
                headers["Tags"] = "sun_behind_small_cloud"
            case 2 :
                headers["Tags"] = "sun_behind_large_cloud"
            case 3 :
                headers["Tags"] = "cloud"
            case 45 | 48 :
                headers["Tags"] = "fog"
            case 51 | 53 | 55 :
                headers["Tags"] = "droplet"
            case 61 | 63 | 65 | 81 :
                headers["Tags"] = "cloud_with_rain"
            case 71 | 73 | 75 :
                headers["Tags"] = "cloud_with_snow"
            case 80 :
                headers["Tags"] = "sun_behind_rain_cloud"
            case 82 | 95 | 96 | 99 :
                headers["Tags"] = "cloud_with_lightning_and_rain"


        try:
            response = requests.post(NTFY_SERVER_URL, data=message.encode('utf-8'), headers=headers)
            response.raise_for_status()
            print(f"Notification envoyée avec succès. Statut : {response.status_code}\n")
            return True
        except requests.exceptions.RequestException as e:
            print(f"Erreur lors de l'envoi de la notification ntfy : {e}")
            print("Vérifiez que votre NTFY_TOPIC est correct et que vous êtes abonné sur votre iPhone.")
            return False

    def main() -> None:
        print("Démarrage du script de notification météo...")

        weather_data = get_weather_forecast()

        if weather_data:
            try:
                title, body, weather_code = extract_daily_summary(weather_data)
                print(f"---- {title} ----")
                print(f"{body}")
                send_ntfy_notification(title, body, weather_code)

            except Exception as e:
                print(f"Une erreur inattendue est survenue lors du traitement des données : {e}")
                send_ntfy_notification("Erreur Script Météo", "Le script a échoué lors du traitement des données. Voir les logs du Pi.")
        else:
            send_ntfy_notification("Erreur Script Météo", "Impossible de récupérer les données météo. Vérifiez la connexion Internet.")

    if __name__ == "__main__":
        main()
    ```

3. **Ajustez la configuration :**

    * Remplacez `NOM_DU_TOPIC` par le nom de votre topic secret ntfy.
    * Ajustez `LATITUDE` et `LONGITUDE` si vous ne souhaitez pas les prévisions pour Montréal.
    * Vérifiez le `timezone` et ajustez-le si nécessaire pour correspondre à l'heure locale de votre machine.

4. **Rendez le script exécutable :**

    ```sh
    chmod +x meteo_notifier.py
    ```

### Test du Script

Exécutez le script manuellement pour vérifier qu'il fonctionne et que vous recevez la notification sur votre iPhone :

```sh
python3 meteo_notifier.py
```

Si tout est correct, vous devriez voir un message de succès dans le terminal et recevoir une notification sur votre iPhone.

---

## Automatisation avec Cron

Pour que le script s'exécute tous les matins, nous allons utiliser l'ordonnanceur `cron` du Raspberry Pi.

1. Créez un fichier pour stocker les logs et donnez les permissions en écriture :

    ```sh
    # Creation du fichier
    touch meteo_log.txt

    # Mise à jour des permissions
    chmod 777 meteo_log.txt
    ```

2. Éditez la crontab :

    ```sh
    crontab -e
    ```

3. Ajoutez la ligne suivante à la fin du fichier pour exécuter le script tous les jours à 6h30 du matin (heure locale de votre Raspberry Pi) :

    ```cron
    30 6 * * * /usr/bin/python3 meteo_notifier.py >> meteo_log.txt 2>&1
    ```

    * `30 6 * * *` : Signifie à la 30e minute de la 6e heure (6h30), tous les jours, tous les mois.
    * `/usr/bin/python3` : Chemin complet vers l'interpréteur Python.
    * `meteo_notifier.py` : Chemin complet vers votre script.
    * `meteo_log.txt 2>&1` : Redirige la sortie standard et les erreurs vers un fichier de log, ce qui est crucial pour le débogage des tâches cron.

Le système est maintenant entièrement configuré. Chaque matin à 6h30, votre Raspberry Pi exécutera le script, récupérera les prévisions météo pour 8h et 14h, et vous enverra une notification push sur votre iPhone.

---
