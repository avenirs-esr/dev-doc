---
layout: page
title: Dossier d'Architecture logicielle - module Security - Integration du contrôle d'accès
permalink: /arch-soft-specif-security-rbac-integration-experimentations/
up: ../../arch-soft-specif-index/
page_content_classes: table-container
---
[Retour](arch-soft-specif-security.markdown)<br/>
<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Security - RBAC - Integration.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 15/06/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>


## RBAC - Intégration du contrôle d'accès
### Objectifs
- Valider les modalités d'intégration du contrôle d'accès au niveau des différents composants.
- Avoir une vision précise des coûts, principalement en terme de performances des différentes solutions.
- Faire un focus particulier sur le coût lié à l'utilisation d'un API Manager.

### Solutions possibles, avantages et inconvénients

- Intégration sous forme de librairie pour utiliser le contrôle d'accès directement dans les différents modules sans passer par l'API.
- Utiliser diretement l'API du contrôle d'acces directement sans passer par l'APIManager.
- Réaliser le contrôle au niveau de l'API MAnager via une requête intermédiaire via une configuration de plugin.
- Commencer avoir une idée plus précise des métriques attendues. Elles pourront être utlisées ensuite pour détecter des problèmes éventuels, positionner des alertes, etc.
- Déterminer et réaliser et mesurer les optimisations nécessaires (cache, base de donnée, etc.).


| **Méthode**                                  | **Avantages**                                                                                 | **Inconvénients**                                                                                   |
|----------------------------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **1. Générer une librairie**                 | - Performance optimale (pas de latence réseau).<br>- Logique intégrée directement dans les modules.<br>- Autonomie des modules sans dépendance réseau. | - Maintenance complexe (mise à jour dans tous les modules).<br>- Risque d’incohérence entre versions.<br>- Nécessite de donner un accès direct à la base de données du contrôleur d'accès, ce qui pose des problèmes de sécurité et de gestion des connexions. |
| **2. Appel direct à l'API REST**             | - Logique centralisée.<br>- Maintenance et mise à jour simplifiées.<br>- Modules moins volumineux. | - Dépendance réseau.<br>- Latence accrue pour chaque requête.<br>- Gestion des défaillances réseau. |
| **3. Contrôle via l'API Manager (plugin)**   | - Centralisation totale des règles d'accès.<br>- Aucun impact direct sur les modules.<br>- Possibilité de cache dans le plugin.<br>- Intégration native avec l'API Manager.<br>- Meilleure observabilité grâce au monitoring centralisé et aux outils d'analyse. | - Dépendance réseau.<br>- Latence accrue pour chaque requête.<br>- Gestion des défaillances réseau.<br>- Ajoute une complexité au niveau de l’API Manager. |


**Notes:**
- En première approche la solution de librairie est laissée de côté en raison des difficultés induites en terme de maintenance - gestion des versions notament - et des accès nécessaires à la base de données.
- Il s'agit donc d'effectuer une comparaison entre l'acces direct à l'API du contrôle d'accès et l'intégration au niveau de l'API Manager. La comparaison porte principalement sur le coût en terme de temps de réponse.


## Scénario utilisé
Les test sont réalisé sur l'environement de développement dockerisé :
- Cluster postgres
- CAS & OpenLDAP
- APISIX
- Module avenirs-portfolio-security 
- Une api minimalise de test (2 end points)

**Remarque :** le frontal Apache est écarté.


1. Réaliser l'intégration sous forme de plugin avec APISIX
1. Définir deux end-points de test un qui s'interface avec le contrôle d'access et l'autre non.
1. Définir un test de charge avec locust.
1. Générer des jeux de test via Faker pour simuler une charge croissante.
1. Pour chaque jeu de test, le charger en base de données puis:
   1. Jouer le scénario locust sur le end point qui utilise le contrôle d'accès et recupérer les métriques.
   1. Jouer le scenario locust sur APISIX dont la route redirige vers le endpoint sans controle d'accès.
   1. Interpréter les résultats : latence induites, timeout, etc en fonction des différents niveaux de charge.
   1. Réaliser des optimisations :
      - cache au niveau de l'APISIX
      - Eventuellement :
      - optimiser le code (revue de code ?).
      - Base de données (index ou autre).
   1. Rejouer les tests de charge dans les deux cas
   1. Mesurer les gains éventuels.








<br/>[Retour](arch-soft-specif-security.markdown)