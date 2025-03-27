---
layout: page
title: Security/Privacy by design - Authentification
permalink: /security-by-design-auth/
page_content_classes: table-container
---
<div style="border: 2px dashed #e67e22; background: #fcf3e6; padding: 1em; margin: 1em 0; font-weight: bold; color: #e67e22;">
  ⚠️ Cette section est en cours de construction.
</div>

**Module :** Avenirs-ESR / ePortfolio - Authentification.<br/> 
**Objet :** Document de sécurité du module d'authentification..<br/>

**Révision :** 1.0.0.<br/>
**Date :** 10/04/2024.<br/>
**Auteur :** A. Deman.<br/>
**Commentaire :** Version initiale - Draft

-----

## Contexte
### Ecosystème 

**SASS/On Premise :** 

- Intégré dans une fédération.
- Utilisation du protocole OIDC : recupération d'un acces ticket stocké dans le localstorage utilisé ensuite dans le header des requetes.
- Interfacé avec l'API manager

### Risques 
**L'Authentification et l'Autorisation sont les pierre angulaire de la sécurité : le risque sur ce composant est maximal**
**SASS/On Premise :**
- Vol du ticket.
- Enjeux de confidentialité pour les ordinateurs en libre service.
- ...

### Technologies

- Docker
- Protocole OIDC
  
**Frontend** :
    - Webcomponent: 
      - Webcomponent API / LIT
      - Programmation réactive fonctionnelle
      - typescript
      - SASS
      - [TODO] Compléter liste
  
**Backend** :
    - SpringBoot
      - Spring Security
      - [TODO] Compléter la liste


 ## Composants
 - web component
   - LIT
   - RXJS
   - OIDC provider (CAS pour le dev)
  
## Check lists
- [Security Webapp Check list](check-lists/security-webapp-checklist.markdown)
- [Attack Surface Analysis](check-lists/security-attack-surface-analysis.markdown)

