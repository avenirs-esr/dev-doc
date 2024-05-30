---
layout: page
title: Dossier d'Architecture logicielle - module Front Office
permalink: /arch-soft-specif-front-office/
up: ../arch-soft-specif-index/
page_content_classes: table-container
---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Front Office.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Contexte 
Il s'agit de la partie Front du portfolio industriel, qui permet fourni les interfaces utilisateur.
Plusieurs grands groupes de fonctionalités sont présentes dans ce module :
- Module APC
- Mon Projet de vie
- CV
- Différents widgets de présentation, tels que les cartes mentales.

## Objectifs
Fournir une interface moderne ergonomique. La technologie n'a pas encore été déterminée mais une expérimentation concernant les webcomponents à été menée pour les éléments qui devront être transportables et agnostiques en terme de framework. Cette expérimentation concerne les problématique d'authentification.

## Contraintes
- Accessibilité (RGAA).
- Ergonomie  et expérience utilisateur : éléments clés du projet, déterminant en terme de ressenti des utilisateurs par rapport à la solution mise en place.  
- Ne s'interface qu'avec l'API Manager.

## Principaux composants

{% include img.html
        src="assets/images/architecture-front-office.png"
        alt="Front Office - Main components"
        caption="Front Office - Main components"
%}



<br/><br/>
[TODO] Documenter l'expérimentation autour des webcomponents.