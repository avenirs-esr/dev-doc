---
layout: page
title: Security/Privacy by design - Intégration continue
permalink: /security-by-design-checklist-ci/
page_content_classes: table-container
---
<div style="border: 2px dashed #e67e22; background: #fcf3e6; padding: 1em; margin: 1em 0; font-weight: bold; color: #e67e22;">
  ⚠️ Cette section est en cours de construction.
</div>
**Module :** Avenirs-ESR / ePortfolio - Intégration continue<br/> 
**Objet :** Liste de vérifications à réaliser et d'outils à tester / utiliser dans le cadre de l'intégration continue<br/>

**Révision :** 1.0.0.<br/>
**Date :** 10/04/2024.<br/>
**Auteur :** A. Deman.<br/>
**Commentaire :** Version initiale - Draft

---
[TODO] 
--- 



### Points de vérification
- 
   
## <img width="20px" src="../../../assets/images/prerequisites.svg" alt="target"> Prérequis 
- Aucun.


## <img width="20px" src="../../../assets/images/target.svg" alt="target"> Actions 
 1. Vérifier que les secrets n'ont pas été commités accidentellement via un outil de type  [TruffleHog](https://github.com/trufflesecurity/trufflehog) 
 2. Vérifier les regex pour éviter le [ReDos.](https://owasp.org/www-community/attacks/Regular_expression_Denial_of_Service_-_ReDoS)
 3. Lister les dépendances : creation de SBOMs Software-Bill-Of-Materials
 4. Vérification des dépendances inutiles.
 5. Vérifier les failles de sécurité dans les dépendances.
 6. Possibilité d'intégrer une analyse statique des sources  (SAST ou SCA)?