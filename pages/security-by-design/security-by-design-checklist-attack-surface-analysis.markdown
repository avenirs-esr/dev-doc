---
layout: page
title: Security/Privacy by design - Surface d'attaque
permalink: /security-by-design-checklist-attack-surface-analysis/
page_content_classes: table-container
---
<div style="border: 2px dashed #e67e22; background: #fcf3e6; padding: 1em; margin: 1em 0; font-weight: bold; color: #e67e22;">
  ⚠️ Cette section est en cours de construction.
</div>
**Module :** Avenirs-ESR / ePortfolio - Attack Surface Analysis<br/> 
**Objet :** Check list pour la limitation de la surface d'attaque<br/>

**Révision :** 1.0.0.<br/>
**Date :** 10/04/2024.<br/>
**Auteur :** A. Deman.<br/>
**Commentaire :** Version initiale - Draft

-----
[TODO] 
- Prévoir une vision "attaquant" dans le dossier d'architecture technique.
- Etudier les outils d'aide à l'analyse de la surface d'attaque :
  - [Scope](https://github.com/weaveworks/scope)
  - [ThreatMapper](https://github.com/deepfence/ThreatMapper)
  - Scanners :
    - [OWASP Zed Attack Proxy(ZAP)](https://www.zaproxy.org/)
    - [ Skipfish -  archivé ancien ?](https://code.google.com/archive/p/skipfish/)
    - [Ecsypno anciennement Arachni (Commercial)](https://ecsypno.com/pages/codename-scnr)
 


## Sources
[Attack Surface Analysis Cheat Sheet.](https://cheatsheetseries.owasp.org/cheatsheets/Attack_Surface_Analysis_Cheat_Sheet.html)
A lire : 
- (Measuring Relative Attack Surfaces)[https://www.cs.cmu.edu/~wing/publications/Howard-Wing03.pdf]
- A voir si l'utilisation de métriques est pertinent: [An Attack Surface Metric (2011)](https://www.researchgate.net/publication/260648740_An_Attack_Surface_Metric)

## Identifier les points d'attaque potentiels
- UI : formulaire et champs de saisie.
- HTTP headers.
- Cookies.
- APIs.
- File.
- Database.
- Local storage / indexed database.
- email, notifications, messages in general.
- Runtime argument.
- ...

## Identifier les données manipulées
- c.f. Dictionnaire des données classifié par criticité.

## Points de vérification

-  [TODO]