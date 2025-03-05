---
layout: page
title: Dossier d'Architecture logicielle - module Security - RBAC - Cas de test 2
permalink: /arch-soft-specif-security-rbac-test-case2/
up: ../arch-soft-specif-index/
page_content_classes: table-container
---
[Retour](arch-soft-specif-security.markdown)<br/>
<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Security - RBAC - Test case 2.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 15/06/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## RBAC - Données de test

### Cas de test 1

- L'utilisateur gribonvald est pair de la MES (Mise en situation) mes_0000 pour une période donnée.
- Seuls les accès d'affichage et de commentaire sont autorisés.
- Tout les autres accès sont refusés.
- Vérification du contexte d'application du rôle (la période de validité dans ce cas) par rapport au context d'exécution (la date d'exécution).

Les vérifications sont faites au niveau du contrôleur.

[Test unitaire correspondant](https://github.com/avenirs-esr/avenirs-portfolio-security/blob/main/src/test/java/fr/avenirsesr/portfolio/security/controllers/AccessControlControllerCase2Test.java)

{% include img.html
        src="assets/images/access_control_fixtures-case2.svg"
        alt="Security - RBAC - Test case2"
        caption="Access control - Cas de test 2"
        width="100%"
%}
<br/>[Retour](arch-soft-specif-security.markdown)

