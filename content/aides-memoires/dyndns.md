---
title: "DNS dynamique"
summary: "Tutoriel pour mettre en place un DNS dynamique vers un site web à l'aide de Cloudflare."
date: "2025-11-17"
categories: ["tuto"]
tags: ["dns", "cloudflare", "serveur web", "site web"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Prérequis

Pour mettre en place le DNS dynamique via Cloudflare, vous devez avoir

- Un compte Cloudflare opérationnel
- Un domaine dont vous êtes le propriétaire
- L'adresse IP vers laquelle vous voulez rediriger

---

## Enregistrements DNS

Avant de le rendre dynamique, il faut d'abord paramétrer le DNS.

Pour cela, allez dans l'onglet DNS de votre compte Cloudflare, puis dans la partie *DNS management* cliquez sur *Add record*.  
Il vous sera alors demandé de choisir le type d'enregistrement, son nom ainsi que l'adresse de redirection. Choisissez un enregistrement de type A et entrez le nom de l'enregistrement ainsi que l'adresse voulue puis cliquez sur *Save*.

Vérifiez que le proxy est bien activé avant de valider votre enregistrement DNS.

![Enregistrement DNS](/images/dns-record.png)

Votre nouvel enregistrement doit maintenant apparaître dans la liste sur votre page DNS.

---

## Mise à jour automatique

Maintenant que l'enregistrement est ajouté , le DNS est actif pour cette adresse. Pour le rendre dynamique, il faut automatiser la mise à jour de l'adresse de redirection de l'enregistrement créé.

Pour ce faire nous allons créer un script bash qui va comparer notre adresse actuelle à celle de l'enregistrement DNS grâce à l'API Cloudflare.

### Création du jeton API

Pour commencer, nous allons créer le jeton API qui va permettre d'accéder à l'enregistrement.

Allez dans votre profil Cloudflare puis, dans l'onglet *API Tokens*, cliquez sur *Create Token*. Choisissez le modèle *Edit zone DNS* puis donnez un nom au jeton et ajoutez votre domaine dans la partie *Zone Resources*.

Au final, le jeton a les propriétés suivantes :

- *Permissions* : `Zone` - `DNS` - `Edit`
- *Zone Resources* : `Include` - `Specific Zone` - `nom_de_votre_domaine`

Vous pouvez ensuite cliquer sur *Continue to summary* puis *Create Token*.

{{< alert "circle-info">}}
Retenez bien le nom que vous avez donné au jeton, il sera utilisé dans le script.
{{< /alert >}}

### Création du script bash

Nous allons maintenant créer le cœur de la mise à jour des enregistrements.

Pour commencer, créez le fichier qui va contenir le script bash :

```sh
sudo nano /usr/local/bin/cloudflare_ddns_update.sh
```

Ensuite, ajoutez le code suivant dans le fichier.

```sh
#!/bin/bash

# Configuration
# Remplacer par votre adresse e-mail Cloudflare
CLOUDFLARE_EMAIL="adresse_mail_du_compte"
# Remplacer par votre jeton API Cloudflare
CLOUDFLARE_API_KEY="nom_du_jeton_api"
# Remplacer par votre nom de domaine principal
ZONE_NAME="nom_de_votre_domaine"

# Liste des sous-domaines complets à mettre à jour.
# Ajoutez autant d'enregistrements que vous le souhaitez dans cette liste.
RECORD_NAMES=("enregistrement1" "enregistrement2")

# Fichier pour stocker la dernière adresse IP connue
IP_FILE="/var/tmp/cloudflare_ddns_ip.txt"
# Fichier de log
LOG_FILE="/var/log/cloudflare_ddns.log"

# Fonction de log
log() {
    # Ajoute la date et l'heure à chaque entrée de log
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> "$LOG_FILE"
}

# 1. Obtenir l'adresse IP publique actuelle
# Utilisation de `dig` pour une méthode plus robuste, avec `curl` en secours.
CURRENT_IP=$(dig +short myip.opendns.com @resolver1.opendns.com) || CURRENT_IP=$(curl -s https://api.ipify.org )

if [ -z "$CURRENT_IP" ]; then
    log "ERREUR: Impossible d'obtenir l'adresse IP publique actuelle."
    exit 1
fi

# 2. Vérifier si l'adresse IP a changé
if [ -f "$IP_FILE" ]; then
    LAST_IP=$(cat "$IP_FILE")
else
    LAST_IP=""
fi

if [ "$CURRENT_IP" == "$LAST_IP" ]; then
    log "INFO: L'adresse IP n'a pas changé ($CURRENT_IP). Aucune mise à jour nécessaire."
    exit 0
fi

log "INFO: L'adresse IP a changé: de '$LAST_IP' à '$CURRENT_IP'. Début de la mise à jour DNS."

# 3. Obtenir l'ID de la Zone (une seule fois pour le domaine principal)
ZONE_ID=$(curl -s -X GET "https://api.cloudflare.com/client/v4/zones?name=$ZONE_NAME" \
    -H "X-Auth-Email: $CLOUDFLARE_EMAIL" \
    -H "X-Auth-Key: $CLOUDFLARE_API_KEY" \
    -H "Content-Type: application/json" | grep -oP '(?<="id":" )[^"]*')

if [ -z "$ZONE_ID" ]; then
    log "ERREUR: Impossible d'obtenir l'ID de la Zone pour '$ZONE_NAME'."
    exit 1
fi

# Boucle sur chaque enregistrement à mettre à jour
for RECORD_NAME in "${RECORD_NAMES[@]}"; do
    log "--- Traitement de l'enregistrement: $RECORD_NAME ---"

    # 4. Obtenir l'ID de l'Enregistrement DNS
    RECORD_DETAILS=$(curl -s -X GET "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records?type=A&name=$RECORD_NAME" \
        -H "X-Auth-Email: $CLOUDFLARE_EMAIL" \
        -H "X-Auth-Key: $CLOUDFLARE_API_KEY" \
        -H "Content-Type: application/json" )

    RECORD_ID=$(echo "$RECORD_DETAILS" | grep -oP '(?<="id":")[^"]*')

    if [ -z "$RECORD_ID" ]; then
        log "ERREUR: Impossible d'obtenir l'ID pour '$RECORD_NAME'. Assurez-vous que cet enregistrement de type A existe."
        continue # Passe à l'enregistrement suivant
    fi

    # 5. Mettre à jour l'enregistrement DNS
    UPDATE_RESPONSE=$(curl -s -X PUT "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/$RECORD_ID" \
        -H "X-Auth-Email: $CLOUDFLARE_EMAIL" \
        -H "X-Auth-Key: $CLOUDFLARE_API_KEY" \
        -H "Content-Type: application/json" \
        --data "{\"type\":\"A\",\"name\":\"$RECORD_NAME\",\"content\":\"$CURRENT_IP\",\"proxied\":true,\"ttl\":1}" )

    if echo "$UPDATE_RESPONSE" | grep -q '"success":true'; then
        log "SUCCÈS: Mise à jour de '$RECORD_NAME' avec l'IP '$CURRENT_IP'."
    else
        log "ERREUR: La mise à jour de '$RECORD_NAME' a échoué. Réponse de l'API: $UPDATE_RESPONSE"
    fi
done

# 6. Sauvegarder la nouvelle adresse IP (uniquement si toutes les mises à jour ont été tentées)
echo "$CURRENT_IP" > "$IP_FILE"
log "--- Fin du script ---"

exit 0
```

Vous devez modifier les valeurs suivantes au début du script :

- `CLOUDFLARE_EMAIL` : l'adresse mail du compte Cloudflare que vous utilisez
- `CLOUDFLARE_API_KEY` : le nom du jeton que vous avez créé
- `ZONE_NAME` : le nom de votre domaine (ex : domaine.com)
- `RECORD_NAMES` : les noms des enregistrements à mettre à jour (ex : dev.domaine.com, test.domaine.com, ...)

Ce script fonctionne même si plusieurs enregistrements sont à mettre à jour car une boucle tourne sur la variable `RECORD_NAMES` qui peut contenir plusieurs enregistrements.

Une fois les valeurs modifiées, vous pouvez enregistrer et quitter le fichier.

Il faut pour finir le rendre exécutable avec la commande suivante :

```sh
sudo chmod +x /usr/local/bin/cloudflare_ddns_update.sh
```

### Automatisation de la mise à jour

La dernière partie est l'automatisation de la mise à jour.

Pour cela, nous allons ajouter l'exécution du script à *cron* qui exécute une liste de programme.

Pour accèder au fichier de configuration de *cron*, entrez la commande suivante :

```sh
crontab -e
```

Ensuite, ajoutez cette ligne à la fin du fichier.

```txt
*/5 * * * * /usr/local/bin/cloudflare_ddns_update.sh
```

Cette ligne indique que le programme `/usr/local/bin/cloudflare_ddns_update.sh` doit être exécuté toutes les 5 minutes.

Le DNS dynamique avec Cloudflare est maintenant opérationnel.

---
