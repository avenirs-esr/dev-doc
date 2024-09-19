---
layout: page
title: Dossier d'Architecture logicielle - module Security
permalink: /arch-soft-specif-security/
up: ../../arch-soft-specif-index/
page_content_classes: table-container
---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Security.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Contexte
La prise en compte des problématiques de sécurité est un élément central du projet et s'appuie sur la mise en place d'une démarche d'amélioration continue de type [security by design](../security-by-design/index.markdown).

Le portfolio est amené à devenir un élément central dans la vie des enseignants et des étudiants pour les formations en APC. La compromission d'un compte pourrait donc avoir de très lourdes conséquences que ce soit pour l'activité possessionnelle des enseignants ou la réussite des étudiants et renforcent encore, si besoin était la criticité des problématiques de sécurité.  

## Objectifs
L'objectif est d'assurer le meilleur niveau possible de sécurité et de confidentialité. 
en préventif, un ensemble de procédures et d'outils visent à limiter au maximum les risques, par exemple en intégrant les bonnes pratiques ou en intégrant des audits dans le pipeline d'intégration continue.
Pour le curatif, Il s'agit d'être en mesure de détecter, déterminer l'impact et de corriger le plus rapidement possible les failles de sécurité, les intrusions ou les compromissions.

## Contraintes
Certaines caractéristiques de ce projet rendent les problématiques de sécurité particulièrement complexes :
- Un grand nombre d'utilisateurs potentiels avec des profils variés.
- des fonctionnalités de partage entre utilisateurs sont au coeur du fonctionnement du portfolio. La granularité des partages est assez fine et peut ne concerner qu'un élément spécifique d'un portfolio.
- Un déploiement de type SASS / Hybride. 
- Une forte interconnexion avec des services extérieurs.
- ...

L'envergure du projet impose de mettre en place un système qui permette de suivre et de tracer au plus prêt le fonctionnement de la plateforme. Il faut, par exemple, être en mesure, à un instant t, de lister les permissions accordées à un utilisateur ou les utilisateurs disposant d'une permission donnée. 


## Principaux composants

{% include img.html
        src="assets/images/architecture-security.png"
        alt="Security - Main components"
        caption="Security module - Main components"
%}

## Base de données

Par souci de simplification et d'efficacité, le lien avec la base de données est direct, sans passer par une API hébergée par le module storage. 


{% include img.html
        src="assets/images/architecture-security-db.png"
        alt="Security - Database"
        caption="Security module - Database"
%}


## Contrôle d'accès
Le contrôle d'accès est un élément central pour la gestion de la sécurité et le choix qui est fait est de mettre en place un système basé sur un modèle simple, robuste et éprouvé, RBAC : <br/>
[Rôle-Based Access Control](./arch-soft-specif-security-rbac.markdown). 


## Premières expérimentations 

- [Modèle de données](arch-soft-specif-security-rbac-mcd.markdown)

## Données de test
- [Cas de test 1 - Role owner](arch-soft-specif-security-rbac-test-case1.markdown)