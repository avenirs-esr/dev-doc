---
layout: page
title: Data provisioning architecture
permalink: /provisioning-architecture/
up: ../data-provisioning/

---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture et process d'alimentation des données.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 15/01/2026<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Objectifs
- Intégrer les données établissements depuis différentes sources dans notre modèle de données.
- Gérer les mises en conformités via un processus de normalisation.
- Gérer les données invalides et les alimentation en plusieurs étapes.
- Enrichir les données si nécessaire (e.g. api pour les UAI).
- Détecter les données invalides.
- Permettre l'audit des modifications.

## Contraintes
- Respect du paradigme microservice :  chaque microservice gère en autonomie sa base de données.
- Résilience : être capable de prendre en compte les arrêts de services, être capable de rejouer les requêtes.
- Traçabilité : permettre de suivre les évolutions de données.
- Réversibilité : possibilité de revenir à un état antérieur.

## Proposition : utiliser un broker entre le système de consolidation et les microservices.

{% include img.html
src="assets/images/arch-provisioning-draft.svg"
alt="Proposition d'architecture générale"
width="20%"
caption="Proposition d'architecture générale"
%}