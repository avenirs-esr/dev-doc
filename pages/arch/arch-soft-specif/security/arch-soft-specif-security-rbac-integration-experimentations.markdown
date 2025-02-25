---
layout: page
title: Dossier d'Architecture logicielle - module Security - Integration du contrôle d'accès
permalink: /arch-soft-specif-security-rbac-integration-experimentations/
up: ../arch-soft-specif-index/
page_content_classes: table-container
---
[Retour](arch-soft-specif-security.markdown)<br/>
<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Security - RBAC - Integration.<br/>
<br/>
**Révision :** 1.0.1<br/>
**Date :** 15/06/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version reliée à la méthodologie de tests de charge<br/>
<br/>


## RBAC - Intégration du contrôle d'accès
### Objectifs
- Valider les modalités d'intégration du contrôle d'accès au niveau des différents composants.
- Avoir une vision précise des coûts, principalement en terme de performances des différentes solutions.
- Faire un focus particulier sur le coût lié à l'utilisation d'un API Manager.
- Déterminer, réaliser et mesurer les optimisations nécessaires (cache, reqêtes, base de donnée, etc.).

### Solutions possibles, avantages et inconvénients

- Intégration sous forme de librairie pour utiliser le contrôle d'accès directement dans les différents modules sans passer par l'API.
- Utiliser diretement l'API du contrôle d'acces directement sans passer par l'API Manager (APISIX).
- Réaliser le contrôle au niveau de l'API Manager via une requête intermédiaire via une configuration de plugin.



| **Méthode**                                  | **Avantages**                                                                                 | **Inconvénients**                                                                                   |
|----------------------------------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **1. Générer une librairie**                 | - Performance optimale (beaucoup moins de latence réseau).<br>- Logique intégrée directement dans les modules.<br>- Autonomie des modules. | - Moins propre en termes d'architecture et de découpage par grands modules.<br/>- Maintenance plus complexe (mise à jour dans tous les modules).<br>- Risque d’incohérence entre versions.<br>- Nécessite de donner un accès direct à la base de données du contrôleur d'accès, ce qui pose des problèmes de sécurité et de gestion des connexions.  |
| **2. Appel direct à l'API REST**             | - Logique centralisée.<br>- Maintenance et mise à jour simplifiées.<br>- Modules moins volumineux. | - Latence accrue pour chaque requête. |
| **3. Contrôle via l'API Manager (plugin)**   | - Centralisation totale des règles d'accès.<br>- Aucun impact direct sur les modules.<br>- Possibilité de cache dans le plugin.<br>- Intégration native avec l'API Manager.<br>- Meilleure observabilité grâce au monitoring centralisé et aux outils d'analyse. | - Latence accrue pour chaque requête.<br/>- Ajoute une complexité au niveau de l’API Manager (mais gérable via les conf de plugins réutilisables). |

<br/>
**Notes:**
- En première approche, la solution de librairie est laissée de côté en raison des inconvénients citésdans le tableau.
- Il s'agit donc d'effectuer une comparaison entre l'acces direct à l'API du contrôle d'accès et l'intégration au niveau de l'API Manager. La comparaison porte principalement sur le coût en terme de temps de réponse. La mise au point du plugin peut être difficile car il s'agit d'un script lua utilisé sous forme de fonction serverless. Cette version de plugin pourra être utilisée pouor la mise au point de cas spécifiques si nécessaire.<br/>
L'utlisation de ce plugin pour la définition de routes est tres simple : [voir l'exemple utilisé pour ce test](https://github.com/avenirs-esr/srv-dev/blob/82e7b0d9f769505300bf3e496e79f645aa761a86/services/apisix/scripts/routes/experiments/set-access_control-integration-route.curl.sh#L9){:target="_blank"}.


## Scénario utilisé
Les tests suivent la [méthodologie de tests de charge](../load-tests/#objectifs){:target="_blank"} mise en place. Ils s'appuient également sur  une api minimalise de test :
- Un end point qui s'interface directement avec le contrôle d'accès.
- Un end point pour lequel le contrôle d'accès est remonté au niveau de l'API Manager .

**A noter :** le premier end point peut être testé également suivant 2 modalités, avec ou sans passage par l'API Manager (mais sans que l'API mananger ne réalise le contrôle d'accès).


**Etapes**

1. Réaliser l'intégration sous forme de plugin avec APISIX
1. Définir deux end-points de test un qui s'interface avec le contrôle d'access et l'autre non.
1. Appliquer la [méthodologie](../load-tests/#objectifs){:target="_blank"} retenue pour les tests de charge.
1. appliquer les tests pour les deux modalités d'intégration du contrôle d'accès.
1. Réaliser des optimisations :
      - Ajouter du cache, notamment au niveau d'APISIX.
      - Eventuellement :
      - optimiser le code (revue de code ?).
      - Base de données (index ou autre).
   1. Rejouer les tests.
   1. Mesurer les gains éventuels.


   ## Premiers résultats

Ces premiers resultats sont effectués sur le serveur de dev : ressources limités, connexion par vpn, etc.Si possible, ils seront à confirmer sur une infrastructure plus proche de celle de production. Cependant on peut tout de même comparer les deux approches puisque les tests sont réalisés dans les même conditions.<br/>
Le coût d'intégration au niveau de l'API MAnager est négiligeable sur une infrastructure peu chargée et semble même légèrement plus performant sur une infrastrcture chargé.<br/>

Le côut est lié à l'évaluation du plugin permettant de réaliser l'intégration. La [version du plugin utilisée](https://github.com/avenirs-esr/srv-dev/blob/82e7b0d9f769505300bf3e496e79f645aa761a86/services/apisix/scripts/routes/experiments/set-access-control-plugin.curl.sh#L29){:target="_blank"} pour ces tests est sous optimale : écriture de log et analyse de la réponse json plutot que de se baser sur le status http.<br/>
Son utilisation pour la définition de route au niveau de l'API Manager est quand à elle très simple, [voir la route utilisée pour ces tests.](https://github.com/avenirs-esr/srv-dev/blob/82e7b0d9f769505300bf3e496e79f645aa761a86/services/apisix/scripts/routes/experiments/set-access_control-integration-route.curl.sh#L14){:target="_blank"}<br/><br/>


### 100 utilisateurs

   - 50 utilisateurs concurents 

   **Contrôle d'accès au niveau de la méthode**

| Type | Name                 | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /node-api/ac-inlined | 5405       | 0       | 56.5         | 39       | 195      | 196                  | 22.53 | 0          |
|      | Aggregated           | 5405       | 0       | 56.5         | 39       | 195      | 196                  | 22.53 | 0          |

[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-inlined/m1.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}



  **Contrôle d'accès au niveau de l'API Manager**

| Type | Name                 | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /apisix-gw/ac-in-apim | 5428       | 0       | 57.99        | 41       | 285      | 196                  | 22.65 | 0          |
|      | Aggregated           | 5428       | 0       | 57.99        | 41       | 285      | 196                  | 22.65 | 0          |

[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-in-apim/m1.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}


<br/><br/>
- 100 Utilisateurs concurents

 **Contrôle d'accès au niveau de la méthode**

| Type | Name                 | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /node-api/ac-inlined | 10797      | 0       | 59.47        | 38       | 468      | 196                  | 45.01 | 0          |
|      | Aggregated           | 10797      | 0       | 59.47        | 38       | 468      | 196                  | 45.01 | 0          |


[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-inlined/m1.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}



  **Contrôle d'accès au niveau de l'API Manager**

  | Type | Name                  | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|-----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /apisix-gw/ac-in-apim | 10873      | 0       | 59.59        | 40       | 331      | 196                  | 45.33 | 0          |
|      | Aggregated            | 10873      | 0       | 59.59        | 40       | 331      | 196                  | 45.33 | 0          |


[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-in-apim/m1.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}


<br/><br/>

- 150 Utilisateurs concurents

 **Contrôle d'accès au niveau de la méthode**

| Type | Name                 | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /node-api/ac-inlined | 16151      | 0       | 63.4         | 37       | 518      | 196                  | 67.34 | 0          |
|      | Aggregated           | 16151      | 0       | 63.4         | 37       | 518      | 196                  | 67.34 | 0          |


[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-inlined/m1.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}



  **Contrôle d'accès au niveau de l'API Manager**

| Type | Name                  | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|-----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /apisix-gw/ac-in-apim | 16218      | 0       | 62.63        | 39       | 639      | 196                  | 67.61 | 0          |
|      | Aggregated            | 16218      | 0       | 62.63        | 39       | 639      | 196                  | 67.61 | 0          |

 
[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-in-apim/m1.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}


<br/><br/>
### 500 utilisateurs
- 50 utilisateurs concurents 

 **Contrôle d'accès au niveau de la méthode**

| Type | Name                 | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /node-api/ac-inlined | 5226       | 0       | 78.68        | 40       | 852      | 196                  | 21.83 | 0          |
|      | Aggregated           | 5226       | 0       | 78.68        | 40       | 852      | 196                  | 21.83 | 0          |


[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-inlined/m5.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}



  **Contrôle d'accès au niveau de l'API Manager**

| Type | Name                  | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|-----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /apisix-gw/ac-in-apim | 5417       | 0       | 58.02        | 40       | 352      | 196                  | 22.6  | 0          |
|      | Aggregated            | 5417       | 0       | 58.02        | 40       | 352      | 196                  | 22.6  | 0          |


[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-in-apim/m5.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}


<br/><br/>

- 100 utilisateurs concurents 

 **Contrôle d'accès au niveau de la méthode**

| Type | Name                 | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /node-api/ac-inlined | 10334      | 0       | 89.55        | 39       | 1234     | 196                  | 43.09 | 0          |
|      | Aggregated           | 10334      | 0       | 89.55        | 39       | 1234     | 196                  | 43.09 | 0          |

[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-inlined/m5.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}



  **Contrôle d'accès au niveau de l'API Manager**

| Type | Name                  | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|-----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /apisix-gw/ac-in-apim | 10830      | 0       | 59.99        | 38       | 426      | 196                  | 45.17 | 0          |
|      | Aggregated            | 10830      | 0       | 59.99        | 38       | 426      | 196                  | 45.17 | 0          |


[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-in-apim/m5.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}


<br/><br/>

- 150 Utilisateurs concurents

 **Contrôle d'accès au niveau de la méthode**

| Type | Name                 | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /node-api/ac-inlined | 15919      | 0       | 75.46        | 37       | 767      | 196                  | 66.36 | 0          |
|      | Aggregated           | 15919      | 0       | 75.46        | 37       | 767      | 196                  | 66.36 | 0          |

[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-inlined/m5.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}



  **Contrôle d'accès au niveau de l'API Manager**

| Type | Name                  | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS   | Failures/s |
|------|-----------------------|------------|---------|--------------|----------|----------|----------------------|-------|------------|
| GET  | /apisix-gw/ac-in-apim | 16235      | 0       | 60.87        | 37       | 379      | 196                  | 67.69 | 0          |
|      | Aggregated            | 16235      | 0       | 60.87        | 37       | 379      | 196                  | 67.69 | 0          |

 
[(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/ac-integration/ac-in-apim/m5.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}


#### Notes pour la faciliter la reprise des tests

<br/>[Retour](arch-soft-specif-security.markdown)