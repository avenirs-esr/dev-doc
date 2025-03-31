---
layout: page
title: Security/Privacy by design - introduction
permalink: /security-by-design-introduction/
page_content_classes: table-container
---
<div style="border: 2px dashed #e67e22; background: #fcf3e6; padding: 1em; margin: 1em 0; font-weight: bold; color: #e67e22;">
  ⚠️ Cette section est en cours de construction.
</div>

**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Document de sécurité du projet.<br/>

**Révision :** 1.0.0<br/>
**Date :** 10/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale - Draft<br/>

-----

## Source
La source d'information principale de ce document est la fondation [OWASP](https://cheatsheetseries.owasp.org) : Open Worldwide Application Security Project. 

## Contexte
La sécurité et la confidentialité sont devenus des enjeux centraux de tout projet informatique, particulièrement dans le contexte actuel. Le projet Avenirs-ESR / ePortfolio est particulièrement concerné par ces problématiques : il sera proposé avec un hébergement multi-établissements en mode SASS/hybride, concernera potentiellement un une très large communauté d'utilisateurs aux profiles variés et sera interconnecté avec un grand nombre de services extérieurs.

L'objectif de ce document est de mettre en place une stratégie basée sur une approche d'**amélioration continue** afin de traiter les problématiques de sécurité et de confidentialité dans le cadre du projet Avenirs-ESR / ePortfolio industriel.

## Introduction
Le concept de Security / Privacy by design consiste à intégrer les questions relatives à la sécurité et à la confidentialité dès les premières phases d'un projet informatique. 
Les gains d'une telle démarche sont multiples :
- Meilleure efficacité.
- Réduction des coûts.
- Meilleure réactivité en cas de problème : détection, compréhension, correctifs, etc.
- ...

## Principes
La démarche de Security / Privacy by Design s'appuie sur différents axes / concepts :

- **Limiter la surface d'attaque :** il peut s'agir, par exemple, des protocoles, des ports ouverts, des outils disponibles (e.g. compilateurs).

- **Principe de moindre privilège / séparation des privilèges :** Least Privilege principle / Separation of Duties . Les droits attribués aux entités (utilisateurs, autres systèmes) doivent être limités par rapport aux tâches ou actions à réaliser. La séparation des privilèges consiste à s'assurer qu'une seule entité n'a pas le contrôle de l'ensemble des étapes d'un processus. 

- **Principe de zero confiance (Zero Trust) :**  on ne peut pas faire confiance à priori aux entités, qui doivent être authentifiés avant d'autoriser des accès.

- **Avoir une stratégie de sécurité ouverte :** ne pas se baser sur la dissimulation, mais sur la qualité des procédures. Prendre conseils auprès d'experts.

- **Réaliser des audits et des tests :** des audits et tests (intrusion, charge, etc.) doivent être réalisés régulièrement. Un partie de ces tests seront réalisés automatiquement via les tests unitaires ou le pipeline d'intégration continue.

- **Défendre en profondeur :** faire reposer la sécurité sur une articulation de différentes mesures plutôt que sur un élément isolé : gestion des erreurs, validation/limitation des saisies utilisateur (e.g. utilisation d'une sanitizer), gestion des logs, etc.

Ces principes sont intégrés à chaque phase du développement : passage du "Software Development Lifecycle" au "Secure Development Lifecycle" (ou du DevOps au DevSecOps).
Il s'agit des grands axes non exhaustifs et on pourrait ajouter, par exemple, le fait de favoriser la simplicité. 


## Démarche
L'architecture du projet est en cours d'élaboration mais correspondra très probablement à un découpage en grands modules fonctionnels : authentification, stockage, etc.
La démarche proposée s'applique suivant deux granularités :
- le projet dans son ensemble,
- au niveau d'un module spécifique,

Et intègre deux modes de déploiement :
- On premise,
- SASS.


### Méthodologie / organisation pour la mise en place de la démarche
- Définir les étapes de mise en place de la stratégie de sécurité.
- Décliner ces étapes en mode opératoire général : e.g. utiliser un outil d'audit des dépendances (sans préciser lequel).
- Proposer un modèle de document de sécurité attaché au projet et à chaque grand module.
- Définir les procédures / points de contrôle : e.g. : fréquence des révisions, réalisation d'audits, réévaluation des risques, désignation (ou non) de référents, etc.
- Définir et documenter les éléments factorisables dans la démarche :
    - check lists par catégories à réutiliser dans les documents de sécurité : technologie utilisée, saisies utilisateur, enregistrement en base de données, etc. Il peut s'agir de point d'attention particuliers, de bonnes pratiques, d'outils, etc.
    - Matrice des rôles/privilèges,
    - ...


### Étapes
**1 - Définition du contexte :**  dans quel écosystème s'inscrit le projet / module, qui l'utilise et pourquoi, quelles sont les données manipulées, les interactions, les technologies employées, quels sont les risques et évaluation des menaces. *(expliciter différences On premise vs SASS)*

**2 - Choix et analyse des composants :** composants, bibliothèque ou services externes. Lister les composants de l'application et évaluer leur sécurisation. Limiter l'usage des bibliothèques et services extérieurs autant que possible.

**3 - Connexions :** décrit comment l'utilisateur interagit avec l'application et comment elle utilise les composants et services, ou est stockée la donnée et comment elle est récupérée. Il peut être intéressant également d'expliciter les connexions qui ne sont volontairement pas mises en place. *(expliciter différences On premize vs SASS)*

**4 - Vérification du code** : liste de points à vérifier, validation des saisies utilisateur, gestion des erreurs, etc. Voir également [Top 10 Web Application Security Risks.](https://owasp.org/www-project-top-ten/)

## TODO
Vérifier si le (Software Assurance Maturity Model)[https://owaspsamm.org/guidance/quick-start-guide/] pourrait être applicable déclinable. En première lecture cela semble un peu compliqué.

## A propos des API managers
L'utilisation d'un API Manager s'inscrit parfaitement dans une démarche de type security by design. Ces outils offrent des fonctionnalités qui permettent de faciliter grandement l'implémentation d'une stratégie de sécurité :
- authentification / autorisation,
- Gestion des flux, limites, cadençage,
- Alertes,
- Audit,
- etc.

## Big picture

{% include img.html
        src="assets/images/security-by-design-big-picture.svg"
        alt="Security by design - Big Picture"
        width="50%"
%}

## Références

- OWASP: 
	- https://owasp.org/www-project-developer-guide/draft/
	- https://cheatsheetseries.owasp.org/index.html
		+ https://cheatsheetseries.owasp.org/cheatsheets/Secure_Product_Design_Cheat_Sheet.html
	- https://owaspsamm.org/model/
    	- https://drive.google.com/file/d/1cI3Qzfrly_X89z7StLWI5p_Jfqs0-OZv/view
      	- https://owaspsamm.org/guidance/quick-start-guide/
- Généralistes:
  - https://www.softfluent.fr/blog/security-by-design-en-general-et-owasp-en-particulier- -
  - https://www.ensta-bretagne.fr/fr/vers-plus-de-cybersecurite-avec-une-approche-security-design-reinventee

- Cours : https://secureby.design/
- DeSecOps : https://www.softfluent.fr/blog/devsecops-pourquoi-est-ce-important/