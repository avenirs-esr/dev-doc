---
layout: page
title: Dossier d'Architecture logicielle - module Communication - Etude préliminaire
permalink: /arch-soft-specif-communication-preliminary-study/
up: ../../arch-soft-specif-index/
page_content_classes: table-container
---
[Retour](arch-soft-specif-communication.markdown)<br/>
<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Module Communication - Notifications et temps réel - Etude préliminaire.<br/>
<br/>
**Révision :** 1.0.1<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Refactoring lors de la mise en oeuvre du syteme de notification et temps réel : séparation des parties etudes préliminaire et mise en oeuvre.**<br/>
<br/>

## Contexte
Etude préliminaire sur la faisabilité et les différentes possibilités d'architecture pour la partie notification et temps réel.

## Objectifs 
Déterminer la faisabilité, les contraintes techniques et servir de point de départ pour la mise en peuvre d'une architecture de notification et temps réel.

## Notifications
Une expérimentation a été menée concernant les notifications temps réel autour de Kafka, APISIX et une API développée Java/Spring Boot.<br/>
Remarques : les fonctionnalités d'APISIX liées à Kafka sont trop limitées pour notre cas d'utilisation (c.f. [GitHub issue](https://github.com/apache/apisix/issues/10947#issuecomment-2014182230)).  

{% include img.html
        src="assets/images/architecture-communication-notifications.png"
        alt="Communication - Notifications"
        caption="Communication - Notifications"
%}

## Modification des données et temps réel

**Cas d'usage :** 
- prendre en compte au niveau UI des modifications de données, ciblées pour les utilisateurs. 
- au niveau systeme prendre en compte les modifications de privileges par exemple.

**Notes :** ne couvre pas l'envoi de notification d'utilisateur à utilisateur mais peut servir de mécanisme de base sur lequel appuyer cette fonctionnalité.

**Intérêt :** Pas de logique à reproduire au niveau des services associés aux features, logique unifiée quel que soit le système qui interragit avec la base de données, couplage fort entre la donnée et le temps réel. 

### Possibilités de mise en oeuvre

**Objectif :** rendre Postgres réactive pour pouvoir déclencher des traitements lors de la création/modification/suppression de données en base. 

**Approches envisagées :**
- Utiliser le mécanisme [Notify](https://www.postgresql.org/docs/current/sql-notify.html) / [Listen](https://www.postgresql.org/docs/current/sql-listen.html). Trop limité pour ce cas d'usage :
  - taille de messages limitée,
  - pas de vérification de livraison,
  - non adapaté à une mise à l'échelle (les perf se dégradent si le volume augmente),
  - mode broadcast uniquement,
  - nécessité de mettre en place des triggers assez complexes pour constituer le paylod,
  - pas de possiblité native de connaitre la valeur précédente pour les modifications/suppression (à gérer dans les triggers si c'est possible).<br/>
- Utiliser [Debezium](https://debezium.io/) afin de s'appuyer sur le mécanisme de [réplication logique](https://docs.postgresql.fr/10/logical-replication.html) de Postgres qui utilise un système de publication/abonnement. Debezium est connecté à postgres et à Kafka pour emettre les changements de données dans les topics spécifiques associés aux tables impliquées. Les topics créés et alimentés pas Debezium sont ensuite consommé soit directement par l'APS de notification soit par Redis ou un équivalent qui va s'interfacer avec l'API de notification. L'intérêt de cette deuxième approche apporte plus de robutesse et de résilience : mise à l'échelle horizontale, possiblité de rejouer les notifications, etc, mais au coût d'une plus grande complexité.
<br/>
**Remarque :** s'il est nécessaire d'avoir les informations concernant les données avant modifications, pour les update et delete, il faut paramétrer postges : 
```
ALTER TABLE nom_de_la_table REPLICA IDENTITY DEFAULT;
```
En pratique, une ligne en plus est écrite dans le WAL avant d'effectuer la modification, ce qui peut avoir un impacte sur les performances.

<br/>
### Shéma d'architecture
**Remarque :** L'API Manager n'est pas représenté. Son rôle, dans cette architecture, est à déterminer, notament par rapport au flux SSE.

{% include img.html
        src="assets/images/architecture-realtime-debezium.sans-redis.svg"
        alt="Notification et temps réel - Architecture sans Redis"
        caption="Notification et temps réel - Architecture sans Redis"
        width="50%"
%}
<br/>
Cette architecture pourrait constituer un point de départ et évoluer enuite vers une architecture plus complète telle que celle représentée ci-dessous.

<br/><br/>

{% include img.html
        src="assets/images/architecture-realtime-debezium.avec-redis.svg"
        alt="Notification et temps réel - Architecture avec Redis"
        caption="Notification et temps réel - Architecture avec Redis"
        width="50%"
%}

<br/>
### Communication vers le client

L'envoi des notifications vers le client peut se faire par web sockets ou en utilisant les [Server-Sent Events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events.)
<br/>

**Limitations du SEE par  rapport aux web sockets :**
- Unidirectionnel : pas de réponse du client donc mécanisme d'acquitement à prévoir si nécessaire.
- Transporte uniquement du text (pas de binaire ou de sous protocole).
- Moins de fonctionnalités (e.g.:négotiation), mais à priori, celles présentes devraient suffire. 
- Un peu plus de latence.

**Avantages du SSE par rapport aux web sockets :**
- Protocole http : moins de risque de blockage sur des routeurs, load balancers, etc.
- Plus légér que les web sockets, en terme de bande passante et de ressources serveur (sur le papier, à confirmer).
<br/>
Dans les deux cas, il possible de transmettre un token d'authentification qui pourra être utiliser pour le contôle d'accès et le routage des notifications.

**Considérations par rapport à la consomation de ressources :**
- Il pourrait être intéressant de mettre en place des mécanismes qui permettent de limiter les consommations. Exemples :
   - Déconnexion si client inactif pendant n minutes puis reconnexion automatique à la reprise d'activité.
   - Regrouper les notifications.
   - Stratégie de communication en deux temps : notifier le besoin de mise à jour mais sans données et le client effectue ensuite une requête traditionnelle pour récupérer les données. 
- Prévoir une stratégie de repli, par exemple long / short polling.
- Avoir la possiblité de désactiver complètement le système de notifications sans compromettre le fonctionnement global de l'application.

