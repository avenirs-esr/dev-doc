---
layout: page
title: Dossier d'architecture logicielle
permalink: /arch-soft-specif-index/
up: ../arch/
---


{% include img.html
        src="assets/images/architecture-visuel-index.png"
        alt="Architecture"
        
%}

## Objet
Le but de ce dossier est de décrire l'architecture logicielle du  portfolio suivant deux axes principaux :
* Une vue d'ensemble des composants du système et de leurs interactions.
* Une présentation plus détaillée de la structure et des spécificités de chacun des modules.<br/>

Les questions relatives à la sécurité et  à la confidentialité sont intégrées dès les premières étapes de conception.

L'objectif final est d'aboutir à un système le plus fiable et évolutif possible, en conservant la maîtrise de la compréhension de l'ensemble du système. <br/>
Cette maîtrise doit être maintenue malgré une complexité et une richesse fonctionnelle importante.<br/>
Documenter les choix structurants est important afin de pouvoir les évaluer et, le cas échéant revenir dessus au cours de la vie du logiciel.<br/>




## Documents d'architecture des principaux modules

[Vue d'ensemble ](../arch-soft-specif-all) : vue d'ensemble des principaux modules et de leurs interactions.

[Back Office](../arch-soft-specif-back-office) : module d'administration, API ePortfolio et gestion de l’interopérabilité.

[Security](../arch-soft-specif-security) : gestion de l'authentification et des contrôles d'accès.

[Data storage](../arch-soft-specif-storage): stockage de données structurées/semi/non structurées. Gestion du cache, de l'indexation et du cycle de vie des données.

[Monitoring](../arch-soft-specif-monitoring) : gestion des statistiques d'usage, des logs, des learning analitics.

[Communication](../arch-soft-specif-communication): notifications utilisateur/système et communication entre utilisateurs.

[Front Office](../arch-soft-specif-front-office): contient les interfaces utilisateurs et les règles métier par grand domaine : APC, Projet de vie, CV, etc.

[API Manager](../arch-soft-specif-apim): il s'agit d'un module particulier, non développé, mais paramétré. Il s'interface avec l'ensemble des API internes qui doivent être exposées et facilite la prise en compte des problématiques de sécurité et d'exploitation. 



