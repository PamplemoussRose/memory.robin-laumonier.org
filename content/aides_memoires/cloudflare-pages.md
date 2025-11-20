---
title: "Cloudflare pages"
summary: "Tutoriel pour déployer un site web via les pages Cloudflare."
date: "2025-11-19"
categories: ["tuto"]
tags: ["site web", "cloudflare", "github"]
layout: "page"
draft: "false" # Set to true if this page is not to be show
---

---

## Prérequis

Ce tutoriel permet de mettre en place un site web à l'aide des pages Cloudflare.  
Je vais utilisé un site généré par le framework Hugo ([**Voir tuto ici**](https://memory.robin-laumonier.org/aides_memoires/hugo/)). Certaines étapes vont être spécifiques à Hugo et seront à adapter selon votre téchnologies.

Pour ce tuto vous devez remplir les conditions suivantes :

- Avoir un compte Cloudflare opérationnel
- Avoir un compte Github opérationnel
- Avoir un dépôt Github avec le code du site à déployer

## Liaison du dépôt Github

La première chose à faire est de lier un dépôt Github à la nouvelle page.

Pour ce faire, allez sur votre compte Cloudflare, rubrique *Build*, page *Workers & Pages*.

Ensuite cliquez sur *Create application*, allez dans la catégorie *Pages* puis séléctionnez *Import an existing Git repository*.

Vous devez maintenant lier votre compte Github à votre compte Cloudflare. Vous pouvez slélctionner les dépôt qui vont être visibles par Cloudflare pour déployer les sites ou applications.

---

## Déploiement du site

Vous pouvez maintenant séléctionner le dépôt contenant votre site à déployer.

Ensuite vous devez nommer votre projet coté Cloudflare et séléctionner la branche du dépôt qui sera utilisée.

La deuxième partie de la page concerne le build du site.  
Vous devez spécifier la commande de build de votre projet ainsi que le dossier de sortie de la commande. Comme j'utilise Hugo, la commande de build est `hugo --gc --minify` et le dossier de sortie est `public`.

{{< alert >}}
**Attention!** Ces deux paramètres sont à ajuster selon la téchnologie utilisée pour faire votre site !
{{< /alert >}}

Une fois que les champs sont remplis, cliquez sur *Save and deploy*. Cloudflare va alors éxécuter la commande que vous lui avez donné à la racine de votre dépôt Github. Si le build échoue, vous aurez un message d'erreur pour vous aider à corriger le problème. Si le build est valide, vous aurez alors un URL qui est l'adresse à laquelle est déployé votre projet.

Si vous possedez un domaine, vous pouvez ajouter votre domiane aux adresses pour accèder à votre projet déployé.

---

## Mise à jour du site

La mise à jour du site est très simple. A chaque fois qu'il y a une mise à jour sur le dépôt Github, Cloudflare va rejouer le script de build et déployer votre projet. Vous n'avez rien de plus à faire, si ce n'est vérifier que le build s'est dérouler correctement.

---
