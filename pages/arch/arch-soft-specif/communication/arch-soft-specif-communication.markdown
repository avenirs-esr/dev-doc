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
**Commentaire :** séparation en 2 parties, étude préliminaire et mise en oeuvre<br/>
<br/>

## Contexte
Module de communication du portfolio industriel, qui permet de gérer les notifications utilisateur, les notifications système et la communication entre utilisateurs. 

## Objectifs 
- Faciliter les usages : 
  - Notamment autour des Activités de Mise en Situation ou de l'évaluation par des pairs. 
  - Permettre aux utilisateurs d'avoir accès à la bonne information au bon moment, sans être noyé par un flux trop important de messages.
  - Guider les utilisateurs avec une communication claire, contextualisée et des indications pertinentes.
- Servir de base technique pour la mise en place de fonctionnalités temps réel.


## Contraintes
- Les notifications doivent être en temps réel.
- Le service doit pouvoir être contrôlé finement et les limites doivent être bien positionnées pour éviter le SPAM et les usages détournés ou abusifs. Il peut s'agir, par exemple, d être en mesure de mettre hors ligne tout ou partie des fonctionnalités de communication. [TODO] déterminer le périmètre : ensemble du système, établissement, catégories d'utilisateurs, utilisateurs spécifiques ?
- Gestion du système de messagerie : possibilité de s'appuyer sur une infra existante pour le service SMTP ? 
- Contrainte de performance, à valider surtout au niveau des notifications système, basées sur les web sockets.
- Maintenir une cohérence, notamment en terme de format de messages sur l'ensemble de la plateforme.

# [Etudes préliminaires](arch-soft-specif-communication-preliminary-study.markdown)
# [Mise en oeuvre](arch-soft-specif-communication-system-integration-plan.markdown)