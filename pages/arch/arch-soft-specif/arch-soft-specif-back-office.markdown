---
layout: page
title: Dossier d'Architecture logicielle - module Back Office
permalink: /arch-soft-specif-back-office/
up: ../arch-soft-specif-index/
page_content_classes: table-container
---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Back Office.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Contexte 
Ce module porte les fonctionnalités d'administration du site, des APIs ePortfolio et de la gestion de l’interopérabilité.

## Objectifs
Les fonctionnalités d'administration doivent permettre aux administrateurs de réaliser les opérations de paramétrage du système. Pour des questions de sécurité, l'interface utilisateur d'administration est séparée de celle du portfolio. 
Cela permettra de réaliser un renforcement de la sécurité, notamment via des restriction d'accès. Cette partie ne concerne que les administrateurs du système portfolio et non les administrateurs pédagogiques. 

Ce module contient également la partie API spécifique ePortfolio, accessible aux utilisateurs via le front office.

Les fonctions d’interopérabilités permettront d'exporter et d'importer les portfolios depuis et vers différents systèmes : 
- portfolio professionnel,
- portfolio scolaire,
- Karuta, etc.
  
L'objectif principal est que le portfolio d'un utilisateur puisse le suivre au cours des différentes étapes de sa vie.<br/>
L'import des utilisateurs doit permettre de créer les comptes du portfolio.

## Contraintes
La première contrainte concerne la sécurité. Les fonctionnalités associées à l'administration du site sont très sensibles et doivent être sécurisées au maximum et tracées.
La partie interopérabilité est très dépendantes des systèmes et des implémentations en place dans les établissements. Ce qui est envisagé est de fournir des scripts ou demi-clients adaptables pour faciliter le travail des établissements.

Il pourrait être intéressant de développer un format d'export normalisé qui pourrait devenir un standard de fait. Ce point est évoqué dans le dossier des fonctionnalités attendues du wp1.

Les services peuvent être accessibles en mode SASS ou directement dans les établissements. Un catalogue de services doit permettre de déterminer les modalités d'accès à un service donné.

L'importation doit être réalisée à la demande pour un utilisateur ou un groupe d'utilisateurs par un administrateur pédagogique ou le responsable d'une formation. L'objectif est de ne pas charger la plateforme inutilement.

## Principaux composants

{% include img.html
        src="assets/images/architecture-back-office.png"
        alt="Back Office - Main components"
        caption="Back Office - Main components"
%}


## Catalogues de services.

Certains services dans les établissements, tels que PStage, POD ou les LMS, devront pouvoir être accédés par le portfolio. Pour déterminer les informations relatives aux modalités d'accès, un catalogue de services sera mis en place.
Ce catalogue doit s'interfacer souplement avec l'API manager pour adapter les requêtes aux spécificités de l'établissement.

Une expérimentation à été réalisée avec Apisix afin de vérifier que les capacités de l'outil permettent de répondre à ce besoin. Cette expérimentation correspond à la branche [apisix-dynamic-upstream](https://github.com/avenirs-esr/srv-dev/blob/apisix-dynamic-upstream/) dans le dépôt GitHub. Voir, notamment, le [script de définition de la route APISIX.](https://github.com/avenirs-esr/srv-dev/blob/apisix-dynamic-upstream/services/apisix/scripts/routes/experiments/dynamic-upstream.curl.sh)

Le schéma suivant décrit cette expérimentation qui consiste en un Mock du catalogue de services et le paramétrage de 4 plugins d'APISIX :
- **Openid-connect :** vérifie qu'un accès token valide est transmis avec la requête
- **Serverless-pre-function :** permet d'exécuter du code LUA avant le traitement de la requête. Ce code interagit avec le catalogue de services pour terminer les caractéristiques du point d'accès sur la base de l'utilisateur associé à l'access token.
- **Trafic split :** permet de sélectionner l'upstream défini dans APISIX sur la base des informations transmises par le catalogue de services.
- **Proxy rewrite :** prise en compte du point d'accès associé au service.

{% include img.html
        src="assets/images/architecture-back-office-services-catalog.png"
        alt="Back Office - Services catalog experiment"
        caption="Back Office - Services catalog experiment with APISIX."
%}


## Résultats de l'expérimentation

**Points positifs :**
- Peut être réalisé avec les plugins natifs d'APISIX.
- Un seul point d'accès par type de service.
- Permet de traiter l'ensemble des requêtes via l'API Manager.

**Points négatifs :**
- La partie serverless est à réaliser en LUA et n'est pas très user friendly à mettre en oeuvre. 
- L'ensemble des upstreams, c'est à dire les adresses et ports des serveurs doivent êtres connus et paramétrés à l'avance. Les URI des services peuvent être dynamiques mais pas les noeuds d'accès (addresses, ports, etc.) 

**Points à vérifier :**
- Tenue à la charge : à vérifier, notamment lié à la fonction serverless. [TODO] déterminer la méthodologie pour les stress tests.
- Gestion des erreurs, des problèmes transitoires de connexion aux services. N.B. : indépendant de l'implémentation retenue.

**Solution de replis si nécessaire :**
Si nécessaire, la logique d'aiguillage et de réalisation de la requête pourrait être faite directement au niveau du catalogue de services. L'inconvénient est qu'une partie de la requête n'est plus traitée via l'API manager, on perd donc en cohérence ainsi qu'en terme d'utilisation des fonctionnalités d'APISIX.


