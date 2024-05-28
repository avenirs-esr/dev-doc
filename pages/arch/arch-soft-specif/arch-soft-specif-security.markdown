---
layout: page
title: Dossier d'Architecture logicielle - module Security
permalink: /arch-soft-specif-security/
up: ../arch-soft-specif/


---
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Security.<br/>

**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale - Draft<br/>

-----
# Contexte
La prise en compte des problématiques de sécurité est un élément centrale du projet et s'appuie sur la mise en place d'une démarche d'amélioration continue de type [security by design](../security-by-design/index.markdown).

Le portfolio est amené à devenir un élément central dans la vie des enseignants et des étudiants pour les formations en APC. La compromission d'un compte pourrait donc avoir de très lourdes conséquences que ce soit pour l'activité pofessionnelle des enseignants ou la réussite des étudiants et renforcent encore, si besoin était la criticité des problématiques de sécurité.  

# Objectifs
L'objectif est d'assurer le meilleur niveau possible de sécurité et de confidentialité. 
en préventif, un ensemble de procédures et d'outils visent à limiter au maximum les risques, par exemple en intégrant les bonnes pratiques ou en intégrant des audits dans le pipeline d'intégration continue.
Pour le curatif, Il s'agit d'être en mesure de détecter, déterminer l'impact et de corriger le plus rapidement possible les failles de sécurité, les intrusions ou les compromissions.

# Contraintes
Certaines caractéristiques de ce projet rendent les problématiques de sécurité particulièrement complexes :
- Un grand nombre d'utilisateurs potentiels avec des profils variés.
- des fonctionnalités de partage entre utilisateurs sont au coeur du fonctionnement du portfolio. La granularité des partages est assez fine et peut ne concerner qu'un élément spécifique d'un portfolio.
- Un déploiement de type SASS / Hybride. 
- Une forte interconnexion avec des services extérieurs.
- ...

L'envergure du projet impose de mettre en place un système qui permette de suivre et de tracer au plus prêt le fonctionnement de la plateforme. Il faut, par exemple, être en mesure, à un instant t, de lister les permissions accordées à un utlisateur ou les utilisateurs diposant d'une permission donnée. 


## Principaux composants

{% include img.html
        src="assets/images/architecture-security.png"
        alt="Security - Main components"
        caption="Security module - Main components"
%}

## Role Based Access Control (RBAC)
Le contrôle d'accès est un élément central pour la gestion de la sécurité et le choix qui est fait est de mettre en place un système basé sur un modèle simple, robuste et éprouvé, RBAC : Role-Based Access Control. 

### RBAC - Schéma de fonctionnement général
Une action est autorisée ou non en fonction des rôles actifs des utilisateurs, de la ressource impliquée et du contexte d'exécution.
Un rôle peut être assigné à un utilisateur avec un scope qui correspond aux ressources pour lesquelles il s'applique. Il peut être associé également à un contexte d'application, par exemple une période de validité ou un établissement.  

{% include img.html
        src="assets/images/architecture-security-rbac.png"
        alt="Security - RBAC"
        caption="Role Based Access Control"
%}

### RBAC - Exemple pour le partage d'une ressource

{% include img.html
        src="assets/images/architecture-security-rbac-ex.png"
        alt="Security - RBAC"
        caption="RBAC - Partage en lecture"
%}
