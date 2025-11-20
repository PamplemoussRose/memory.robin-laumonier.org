---
title: "Cloudflare Pages"
summary: "Tutoriel pour déployer un site web via les Pages Cloudflare."
date: "2025-11-19"
categories: ["tuto"]
tags: ["site web", "cloudflare", "github"]
layout: "page"
draft: "false" # Set to true if this page is not to be shown
---

## Prérequis

Ce tutoriel permet de mettre en place un site web à l'aide des Pages Cloudflare.  
Je vais utiliser un site généré par le framework Hugo ([**Voir tuto ici**](https://memory.robin-laumonier.org/aides_memoires/hugo/)). Certaines étapes vont être spécifiques à Hugo et seront à adapter selon votre technologie.

Pour ce tutoriel, vous devez remplir les conditions suivantes :

- Avoir un compte Cloudflare opérationnel
- Avoir un compte GitHub opérationnel
- Avoir un dépôt GitHub avec le code du site à déployer

## Liaison du dépôt GitHub

La première chose à faire est de lier un dépôt GitHub à la nouvelle page.

Pour ce faire, allez sur votre compte Cloudflare, rubrique *Build*, page *Workers & Pages*.

Ensuite, cliquez sur *Create application*, allez dans la catégorie *Pages*, puis sélectionnez *Import an existing Git repository*.

Vous devez maintenant lier votre compte GitHub à votre compte Cloudflare. Vous pouvez sélectionner les dépôts qui seront visibles par Cloudflare pour déployer les sites ou applications.

---

## Déploiement du site

Vous pouvez maintenant sélectionner le dépôt contenant votre site à déployer.

Ensuite, vous devez nommer votre projet côté Cloudflare et sélectionner la branche du dépôt qui sera utilisée.

La deuxième partie de la page concerne le build du site.  
Vous devez spécifier la commande de build de votre projet ainsi que le dossier de sortie de la commande. Comme j'utilise Hugo, la commande de build est `hugo --gc --minify` et le dossier de sortie est `public`.

{{< alert >}}
**Attention !** Ces deux paramètres sont à ajuster selon la technologie utilisée pour créer votre site !
{{< /alert >}}

Une fois que les champs sont remplis, cliquez sur *Save and deploy*. Cloudflare va alors exécuter la commande que vous lui avez donnée à la racine de votre dépôt GitHub. Si le build échoue, vous aurez un message d'erreur pour vous aider à corriger le problème. Si le build est valide, vous obtiendrez alors une URL correspondant à l'adresse de déploiement de votre projet.

Si vous possédez un domaine, vous pouvez ajouter votre domaine aux adresses pour accéder à votre projet déployé.

---

## Mise à jour du site

La mise à jour du site est très simple. À chaque fois qu'il y a une mise à jour sur le dépôt GitHub, Cloudflare va rejouer le script de build et déployer votre projet. Vous n'avez rien de plus à faire, si ce n'est vérifier que le build s'est déroulé correctement.

---
