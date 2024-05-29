---
layout: page
title: Dossier d'Architecture logicielle - module Stockage
permalink: /arch-soft-specif-storage/
up: ../arch-soft-specif-index/
page_content_classes: table-container
---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Stockage.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Contexte 
 
Partie Stockage du portfolio industriel, qui permet de gérer le stockage de données structurée, semi ou non structurée ainsi que de la gestion du cache côté backend et l'indexation. [TODO] Déterminer si l'indexation est effectivement nécessaire.

## Objectifs
Ce module doit permettre la persistance des données suivant différentes modalités en fonction de leur type. 
A ce stade plusieurs types de stockages sont envisagés :
[TODO] à vérifier / corriger / compléter
- SGBDR : pour les données structurées, par exemple le paramétrage, les préférences utilisateur. Un cluster PostgreSQL à été mis en oeuvre pour tests dans l'environnement de développement.
- NoSQL : pour les données non structurées, par exemple les métadatas ou des données d'indexation.
- S3 : pour les traces et la gestion des versions.

## Contraintes
* Contrainte de performances et de fiabilité : pas de ralentissement et pas de perte de données. 
* Gestion du cycle de vie des données.
* Certaines données doivent être versionnées, par exemple les traces.
* La question de la restauration des données utilisateurs reste ouverte : quelles données peuvent être restaurées pour un utilisateur à une date donnée ? [TODO] 
* Les données sensibles ne doivent pas être mises en cache. Il peut s'agir, par exemple, du résultat la validation du token d'authentification par le provider OIDC.

## Principaux composants
{% include img.html
        src="assets/images/architecture-data-storage.png"
        alt="Storage - Main components"
        caption="Storage module - Main components"
%}

## Architecture envisagée pour le stockage de données structurées

{% include img.html
        src="assets/images/architecture-data-storage-structured.png"
        alt="Storage - Structured data"
        caption="Storage module - Structured data"
%}
