---
layout: page
title: Dossier d'Architecture logicielle - module Security - RBAC - Cas de test 1
permalink: /arch-soft-specif-security-rbac-test-case1/
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

Les vérifications sont faites au niveau du contrôleur.

#### Remarques 
- Les end points sont sans doute à revoir : un end point pour la création/modification/suppression et méthode http correspondante. Un end point général type "resource" ou plutôt un end point par type de resource ? 
- Le role owner donne toutes les permissions -> pas trop de sens pour feedback ?
- Note: pas de hiérarchie pour les permissions du type delete->edit->display. Le coût est assez limité : quelques enregistrements en base de données pour un gain de simplicité de performance : pas de calcul à faire, on vérifie qu'un liste de permissions accordées contient la liste des permissions requises directement, [cf. RBAC Algorithme.](https://avenirs-esr.github.io/dev-doc/arch-soft-specif-security-rbac/#rbac---algorithme)

[Test unitaire correspondant](https://github.com/avenirs-esr/avenirs-portfolio-security/blob/main/src/test/java/fr/avenirsesr/portfolio/security/controllers/AccessControlControllerCase1Test.java)


{% include img.html
        src="assets/images/access_control_fixtures-case1.svg"
        alt="Security - RBAC - Test case1"
        caption="Access control - Cas de test 1"
        width="100%"
%}
<br/>[Retour](arch-soft-specif-security.markdown)

