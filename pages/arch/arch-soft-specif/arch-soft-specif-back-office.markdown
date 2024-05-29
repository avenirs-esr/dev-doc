---
layout: page
title: Dossier d'Architecture logicielle - module Back Office
permalink: /arch-soft-specif-back-office/
up: ../arch-soft-specif/
page_content_classes: table-container
---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Back Office.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale - Draft<br/>
<br/>

Ce module porte les fonctionnalités d'administration du site, des APIs ePortfolio et de la gestion de l’interopérabilité.

## Objectifs
Les fonctionnalités d'administration doivent permettre aux administrateurs de réaliser les opérations de paramétrage du système. 
Pour des questions de sécurité l'interface utilisateur d'administration est séparée de celle du portfolio. Cela permettra de réaliser un renforcement de la sécurité, notamment via un renforcement des restriction d'accès. Cette partie ne concerne que les administrateur du système portfolio et non les administrateurs pédagogiques. 

Ce module contient également la partie API spécifique ePortfolio, accessible aux utilisateurs via le front office.

Les fonctions d’interopérabilités permettront d'exporter et d'importer les portfolios depuis et vers différents systèmes : 
- portfolio professionnel,
- portfolio scolaire,
- Karuta, etc.
L'objectif principal est que le portfolio d'un utilisateur puisse le suivre au cours des différentes étapes de sa vie.
L'import des utilisateurs doit permettre de créer les comptes du portfolio.

## Contraintes
La première contrainte concerne la sécurité. Les fonctionnalités associées à l'administration du site sont très sensibles et doivent être sécurisées au maximum et tracées.
Pour la partie interopérabilité est très dépendantes des systèmes et des implémentations en place dans les établissements. Ce qui est envisagé est de fournir des scripts ou demi-clients adaptables pour faciliter le travail des établissements.
Il pourrait être intéressant de développer un format d'export normalisé qui pourrait devenir un standard de fait. Ce point êt évoqué dans le dossier des fonctionnalités attendues du wp1.
Les services peuvent être accessible en mode SASS ou directement dans les établissements. Un catalogue de services doit permettre de déterminer les modalités d'accès à un service donné.

L'importation doit être réalisée à la demande pour un utilisateur ou un groupe d'utilisateurs par un administrateur pédagogique ou le responsable d'une formation. L'objectif est de ne pas charger la plateforme inutilement.


## Principaux composants
{% include img.html
        src="assets/images/architecture-security.png"
        alt="Back Office - Main components"
        caption="Back Office - Main components"
%}