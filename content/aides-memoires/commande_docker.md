---
title: "Commandes Docker"
summary: "Mémo des commandes nécessaire pour utiliser docker"
date: "2026-07-09"
categories: ["mémo"]
tags: ["docker", "commande", "shell"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

---

## Images

```sh
docker images                          # lister les images locales
docker pull <image>:<tag>              # télécharger une image
docker build -t <nom>:<tag> .          # construire depuis un Dockerfile
docker rmi <image>                     # supprimer une image
docker image prune -a                  # supprimer toutes les images inutilisées
docker tag <image> <nouveau_nom>:<tag> # renommer/tagger une image
docker save -o fichier.tar <image>     # exporter une image
docker load -i fichier.tar             # importer une image
docker inspect <image>                 # détails complets d'une image
```

---

## Conteneurs

```sh
docker ps                              # conteneurs actifs
docker ps -a                           # tous les conteneurs
docker run <image>                     # créer et démarrer un conteneur
docker run -d -p 8080:80 --name <nom> <image>  # run en arrière-plan
docker start <conteneur>               # démarrer un conteneur arrêté
docker stop <conteneur>                # arrêt propre (SIGTERM)
docker kill <conteneur>                # arrêt forcé (SIGKILL)
docker restart <conteneur>             # redémarrer
docker rm <conteneur>                  # supprimer un conteneur arrêté
docker rm -f <conteneur>               # supprimer de force (même actif)
docker container prune                 # supprimer tous les conteneurs arrêtés
docker rename <ancien> <nouveau>       # renommer un conteneur
docker pause / unpause <conteneur>     # suspendre / reprendre
```

---

## Exécution & Accès

```sh
docker exec -it <conteneur> bash       # shell interactif dans un conteneur actif
docker exec <conteneur> <commande>     # exécuter une commande sans shell
docker attach <conteneur>              # s'attacher au processus principal
docker cp <conteneur>:/chemin ./local  # copier fichier hors conteneur
docker cp ./local <conteneur>:/chemin  # copier fichier dans le conteneur
```

---

## Logs & Monitoring

```sh
docker logs <conteneur>                # afficher les logs
docker logs -f <conteneur>             # suivre les logs en temps réel
docker logs --tail 100 <conteneur>     # 100 dernières lignes
docker stats                           # utilisation CPU/mémoire en temps réel
docker top <conteneur>                 # processus dans un conteneur
docker inspect <conteneur>             # configuration complète JSON
docker diff <conteneur>                # modifications du filesystem
docker events                          # flux d'événements Docker
```

---

## Réseau

```sh
docker network ls                      # lister les réseaux
docker network create <nom>            # créer un réseau
docker network rm <nom>                # supprimer un réseau
docker network inspect <nom>           # détails d'un réseau
docker network connect <réseau> <conteneur>     # connecter
docker network disconnect <réseau> <conteneur>  # déconnecter
docker network prune                   # supprimer les réseaux inutilisés
```

---

## Volumes

```sh
docker volume ls                       # lister les volumes
docker volume create <nom>             # créer un volume
docker volume rm <nom>                 # supprimer un volume
docker volume inspect <nom>            # détails d'un volume
docker volume prune                    # supprimer les volumes inutilisés
```

---

## Docker Compose

```sh
docker compose up -d                   # démarrer les services en arrière-plan
docker compose down                    # arrêter et supprimer les conteneurs
docker compose down -v                 # idem + supprimer les volumes
docker compose ps                      # état des services
docker compose logs -f                 # logs de tous les services
docker compose build                   # reconstruire les images
docker compose pull                    # mettre à jour les images
docker compose exec <service> bash     # shell dans un service
docker compose restart <service>       # redémarrer un service
docker compose scale <service>=3       # scaler un service
```

---

## Système & Nettoyage

```sh
docker system df                       # espace disque utilisé
docker system prune                    # supprimer toutes les ressources inutilisées
docker system prune -a --volumes       # nettoyage complet (images, volumes inclus)
docker info                            # infos sur le daemon Docker
docker version                         # version client/serveur
```

---

## Registry

```sh
docker login                           # s'authentifier
docker logout                          # se déconnecter
docker push <image>:<tag>              # pousser une image vers un registry
docker search <terme>                  # chercher sur Docker Hub
```

---
