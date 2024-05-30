---
layout: page
title: Dossier d'Architecture logicielle - module Monitoring
permalink: /arch-soft-specif-monitoring/
up: ../arch-soft-specif-index/
page_content_classes: table-container
---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Monitoring.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Contexte  
Le module de monitoring est responsable de la gestion des logs, traces, alertes et statistiques d'usage. Il est en lien étroit avec l'API Manager et les outils du Data Lakehouse.

## Objectifs 
Ce module doit fournir les outils permmettant de suivre au plus près le fonctionnement du portfolio avec :
- GLa gestion des logs et le monitoring pour l'exploitation.
- Les statistiques d'usages / learning analitics pour l'ingénierie pédagogique, les tableaux de bord, les remontées d'informations. 

## Contraintes
- Contrainte de sécurité :  les données sensibles ne doivent pas apparaître ni être stockées au niveau de ce module. 
**A tester :** outils  de type Kafka R  (Kafka + RStudio)
## Principaux composants
{% include img.html
        src="assets/images/architecture-monitoring.png"
        alt="Monitoring - Main components"
        caption="Monitoring module - Main components"
%}

