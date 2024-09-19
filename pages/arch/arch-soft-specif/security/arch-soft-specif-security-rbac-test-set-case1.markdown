---
layout: page
title: Dossier d'Architecture logicielle - module Security - RBAC - Données de test Cas 1
permalink: /arch-soft-specif-security-rbac-test-set-case1/
up: ../../arch-soft-specif-index/
page_content_classes: table-container
---
[Retour](arch-soft-specif-security.markdown)<br/>
<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Security - RBAC - Test case 1.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 15/06/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## RBAC - Données de test

### Cas de test 1

- L'utilisateur deman est propriétaire du portfolio ptf_0000
- Tous les accès sont autorisés pour le portfolio ptf_0000 pour l'utilisateur deman.
- tous les accès sont refusés pour un autre portfolio pour l'utilisateur deman.
- tous les accès sont refusés pour le portfolio ptf_0000 pour un autre utilisateur.

Les vérification sont faites au niveau du contrôleur.

#### Remarques 
- Les end points sont sans doute à revoir : un end point pour la création/modification/suppression et méthode http correspondante. Un end point général type "resource" ou plutôt un end point par type de resource ? 
- 


{% include img.html
        src="assets/images/access_control_fixtures-case1.svg"
        alt="Security - Main components"
        caption="Access control - Fixtures"
        width="100%"
%}
<br/>[Retour](arch-soft-specif-security.markdown)

