---
layout: page
title: Interopérabilité - Référentiel Général d'Interopérabilité
permalink: /interoperability-rgi/
up: ../interoperability/

---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Interopérabilité - Référentiel Général d'Interopérabilité.<br/>
<br/>
**Date :** 23/10/2025<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Notes de synthèse

**Ressource :** [Référentiel Général d'Interopérabilité](https://www.numerique.gouv.fr/offre-accompagnement/reference-interoperabilite-rgi/){:target="_blank"}<br/>
**Contexte :** Le RGI s'intègre dans l'EIF : European Interoperability Framework<br/>
La version 2 de ce document date de 2015 (RFC) et a été publiée en 2016.

Ce document concerne les Autorités Administratives (AA), c'est-à-dire les organisations publiques au sens large. <br/>
Il distingue 5 niveaux d'interopérabilité :
- Niveau politique (orientations, vision commune),
- Niveau juridique : cadre légal, accords contractuels,
- Niveau organisationnel : processus, rôles des parties prenantes,
- Niveau sémantique : sens des mots (unité d'information), cycle de vie, règles d'agrégation et de décomposition.   
- Niveau technique : protocoles d'échange,
Il regroupe un certain nombre de recommandations concernant les protocoles à utiliser pour les niveaux d'interopérabilité sémantiques.

La version 2 de ce document introduit la notion de profil d'interopérabilité qui est un regroupement de préconisations en fonction de cas d'usage identifiés :
- A2A : échanges entre Autorités administratives,
- A2B : échanges entre Autorités Administratives et Entreprises (au sens large, concerne également les associations),
- A2C : échanges entre Autorités Administratives et Citoyens.

Les préconisations concernent essentiellement les formats d'échange, les protocoles et standards pour les niveaux sémantiques et techniques. 

## Positionnement de ce document par rapport au projet Cofolio 
- Le document est très daté et les préconisations sont peu utiles dans le cadre du projet Cofolio. Par exemple, pour les web services le protocole SOAP est préconisé, pour le Web le JavaScript est préconisé (pas de référence au TypeScript), le HTTPS est préconisé mais le HTTP aussi, etc.
- Certains concepts restent utiles, tels que :
   - les profils et les niveaux d'interopérabilité, qui peuvent nous aider à positionner le périmètre des mécanismes d'interopérabilité à mettre en place.
   - La notion de format pivot pourrait également être intéressante à reprendre pour documenter le lien entre les objets externes et ceux de Cofolio.  Le format pivot pour les personnes est présenté (idpPers). Il est relatif à la notion d'identité numérique et modélise le minimum d'informations échangées concernant l'identité d'une personne (dans le document, cela correspond à un état civil, probablement à adapter au contexte ESR).