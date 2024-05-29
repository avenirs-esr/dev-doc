---
layout: page
title: Software Architecture Specification
permalink: /arch-soft-specif/
up: ../arch/
---
<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Description de l'architecture logicielle.<br/>
**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Contexte
Il s'agit de redévelopper entièrement une version industrielle d'un ePortfolio sur la base des expérimentations menées sur Karuta et le  modèle KPC+. 

## Objectifs
L'objectif de cette version industrielle est de permettre un passage à l'échelle en terme de charge, d'évolution et d'exploitation. 
Cette version doit pouvoir être déployée en mode SASS multi-établissements ainsi qu'en mode hybride : seuls certains services / réferentiels resteraient centralisés.
La nouvelle version doit reprendre une majeure partie des fonctionalités proposées par Karuta, en les adaptant le cas échéant, ainsi que de nouvelles fonctionnalités. Un focus particulier est porté sur l'ergononomie et l'accessiblité.

## Contraintes
Forte interopérabilité : le nouveau ePortfolio doit être en mesure de communiquer avec 
- Le SI scolarité établissement, Pégase, Apogée. 
- Des services déployés dans les établissement ou en mode SASS, Esup stage, POD, LMS, ...
- Différents référentiels, existants : RNCP, ROM4.0, etc, ou à venir : référentiel de formations modélisées en APC (projet connexe porté par Avenirs).
- Différents portfolios, scolaire, professionnel afin que les utilisateurs puissent conserver leur portfolio au court des différentes étapes de leur vie.
- Karuta pour permette aux établissements de migrer sur le nouveau portfolio.
  
La notion de sécurité est centrale et doit être intégrée à chaque étape du cycle de développement puis d'exploitation du portfolio industriel. Une démarche d'amélioration continue de type [security by design](../security-by-design/index.markdown) est mise en place pour répondre à cet objectif.

## Type d'architecture
L'architecture mise en place devrait combiner plusieurs types d'architecture classiques :
- Client serveur : le frontend est séparé du backend et la communication entre les deux est réalisée par l'intermédiaire d'API. A terme plusieurs clients pourraient être proposés, par exemple web et mobile.
- Architecture modulaire : le découpage est réalisé par grands groupes fonctionnels (cf. section suivante). Ces grands modules doivent viser à être le plus autonomes et découplés possible.
- Architecture pilotée par les évènements : limité au domaine des notifications  et l'aspect réactif des interfaces utilisateur.
- Modèle/Vue/Contrôleur : pour les interfaces utilisateur.

## Vue d'ensemble des principaux modules et de leurs interactions

{% include img.html
        src="assets/images/architecture-components-diagram.png"
        alt="Modules and their interactions"
        width="100%"
        caption="Main modules"
%}

## Liste des modules

[Back Office](./architecture-back-office.markdown) : module d'administration, API ePortfolio et gestion de l'interoperabilité.

[Security](../arch-soft-specif-security) : gestion de l'authentification et des contrôles d'accès.

[Data storage](./architecture-data-storage.markdown): stockage de données structurées/semi/non structurées. Gestion du cache, de l'indexation et du cycle de vie des données.

[Monitoring](./architecture-monitoring.markdown) : gestion des statisiques d'usage, des logs, des learning analitics.

[Communication](./architecture-communication.markdown): notifications utilisateur/système et communication entre utilisateurs.

[Front Office](./architecture-front-office.markdown): contient les interfaces utilisateurs et les règles métier par grand domaine : APC, Projet de vie, CV, etc.

[API Manager](./architecture-apim.markdown): il s'agit d'un module particulier, non développé, mais paramétré. Il s'interface avec l'ensemble des API internes qui doivent être exposées et facilite la prise en compte des problématiques de sécurité et d'exploitation. 



