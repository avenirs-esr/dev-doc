---
layout: page
title: Security/Privacy by design - ePortfolio
permalink: /security-by-design-eportfolio/
page_content_classes: table-container
---
<div style="border: 2px dashed #e67e22; background: #fcf3e6; padding: 1em; margin: 1em 0; font-weight: bold; color: #e67e22;">
  ⚠️ Cette section est en cours de construction.
</div>
**Projet :** Avenirs-ESR / ePortfolio.<br/> 
**Objet :** Document de sécurité du projet.<br/>

**Révision :** 1.0.0<br/>
**Date :** 10/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale - **Draft**

-----



## Contexte
### Ecosystème 
**SASS :** 
- Multitenant : tous les utilisateurs des établissements accèdent à la même plateforme.
- Authentifié via la fédération RENATER
- Géré et monitoré par l'exploitant.

**On Premise :**
- Mono établissement
- Les utilisateurs de l'établissement accèdent à l'instance.
- Des utilisateur extérieurs peuvent y accéder mais de façon plus limitée.

### Risques 
**SASS :**
- Mauvais cloisonnement multitenants, collision de comptes.
- Tenue à la charge / pic d'utilisation.
- Fuite de données confidentielles : traces, échanges utilisateurs, etc. Différents niveaux de confidentialité, e.g.: traces pour un stage dans un contexte de secret défense.
- Usurpation de compte.
- Validation / certification des traces et preuve : les traces et preuves sont des éléments essentiels dans le fonctionnement du portfolio. Par exemple, il pourrait être nécessaire de produite un historique dans le cadre de réclamations.
- Erreurs non traités / incorrectement traitée.
  
**On Premise : **
- Idem SASS (hormis le risque de mauvais cloisonement multitenants).
- Modifications locale: modification du code, des models, limites, etc.

 ## Composants
 La liste des composants correspond aux grands modules fonctionnels de l'application. Se référer aux document de sécurité de chaque module.
 - [Authentification](security-auth.markdown)
 - Stockage,
 - Notifications,
 - [TODO] Compléter la liste des modules

