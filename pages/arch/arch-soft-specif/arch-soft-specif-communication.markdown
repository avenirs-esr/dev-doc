---
layout: page
title: Dossier d'Architecture logicielle - module Communication
permalink: /arch-soft-specif-communication/
up: ../arch-soft-specif-index/
page_content_classes: table-container
---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Communication.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Contexte
Module de communication du portfolio industriel, qui permet de gérer les notifications utilisateur, les notifications système et la communication entre utilisateurs. 

## Objectifs 
- Faciliter les usages : 
  - Notamment autour des Activités de Mise en Situation ou de l'évaluation par des pairs. 
  - Permetre aux utilisateurs d'avoir accès à la bonne information au bon moment, sans être noyé par un flux trop important de messages.
  - Guider les utilisateurs avec une communication claire, contextualisée et des indications pertinentes.
- Servir de base technique pour la mise en place de fonctionnalités temps réel.


## Contraintes
- Les notifications doivent être en temps réel.
- Le service doit pouvoir etre controlé finement et les limites doivent être bien positionnées pour éviter le SPAM et les usages détournés ou abusifs. Il peut s'agir, par exemple, d'être en mesure de mettre hors ligne tout ou partie des fonctonnalités de communication. [TODO] déterminer le périmètre : ensemble du système, établissement, catégories d'utilistateurs, utilisateurs spécifiques ?
- Gestion du système de messagerie : possibilité de s'appuyer sur une infra existante pour le service SMTP ? 
- Contrainte de performance, à valider surtout au niveau des notifications système, basées sur les web sockets.
- Maintenir une cohérence, notament en terme de format de messages sur l'ensemble de la plateforme.


## Principaux composants

{% include img.html
        src="assets/images/architecture-communication.png"
        alt="Communication - Main components"
        caption="Communication - Main components"
%}

## Notifications
Une expérimentation a été menée concernant les notifications temps réel autour de Kafka, APISIX et une API développée Java/Spring Boot.<br/>
Remarques : les fonctionnalités d'APISIX liées à Kafka sont trop limitées pour notre cas d'utilisation (c.f. [GitHub issue](https://github.com/apache/apisix/issues/10947#issuecomment-2014182230)).  

{% include img.html
        src="assets/images/architecture-communication-notifications.png"
        alt="Communication - Notifications"
        caption="Communication - Notifications"
%}
