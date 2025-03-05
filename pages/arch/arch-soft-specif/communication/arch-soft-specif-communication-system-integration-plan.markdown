---
layout: page
title: Dossier d'Architecture logicielle - Module Communication - Plan de mise en oeuvre
permalink: /arch-soft-specif-communication-system-integration-plan/
up: ../arch-soft-specif-index/
page_content_classes: table-container
---
[Retour](arch-soft-specif-communication.markdown)<br/>
<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Module Communication - Notifications et temps réel - Etude préliminaire.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 05/03/2025<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Plan de mise en oeuvre à partir des [études préliminaire.](../arch-soft-specif-communication-preliminary-study/){:target="_blank"}<br/>
<br/>

## Contexte
Dans un premier temps on laisse de côté le partie notification push mobile et la partie mail.<br/>
Les [expérimentations réalisées](../arch-soft-specif-communication-preliminary-study/){:target="_blank"} autour du temp réel et des notifications servent de base à l'élaboration de l'architecture technique.

## Objectifs 
Mettre en place une première version d'architecture scalable qui serve de socle technique pour :
- Les notifications système : e.g. une modification au niveau du contrôle d'accès.
- Le notifications utilisateur (notification push web) : par exemple, dans le cadre d'une mise en situation.
- Le temps réel pour des UI réactives : afficher des changements au niveau des éléments affichés sans action utlisateur coté client pour permettre un meilleur engagement.
- Mettre en place un mécanisme de routage des messages :
    - Envoyer les messages à la bonne audience . Les différentes granularités d'audiences sont à déterminer : utilisateur, groupe d'utilisateurs, types d'utlisateurs (par exemple administrateurs d'établissement), établisssement, formation, etc.
    - Gérer les aquittemnents.
    - Déterminer les stratégies de retry.
- Les stratégies de fallback.
- L'observabilité, dans un deuxième temps car cela dépend des possibilité d'intégration avec Apisix.


## Architecture
- En première approche l'API manager n'est pas intégré dans l'architecture, le support SSE d'Apisix reste à étudier.
- Pour une question de scalabilité, on utilise un worker en nodejs pour ventiler les message kafka vers KeyDB.
- La fonctionnalité stream de KeyDB est utilisée plutôt que le mécanisme de pub/sub pour bénéficier du mécanisme d'acquittement et des possibiltés de retry.
- L'envoie de notification peut être réalisé via deux mécanimes :
    - Une modification dans une table postgres, relayé par Debezium.
    - Une requête au niveau de l'API de notification.

<br/><br/>

{% include img.html
        src="assets/images/arch-soft-specif-communication-system-integration-plan.svg"
        alt="Notification et temps réel - Architecture"
        caption="Notification et temps réel - Composants et flux"
        width="50%"
%}
<br/>
## Schéma des messages (il s'agit d'une base à ce stade)
<br/>
``` 
{

    metadata: {
        "id": "uuid", // UUID de la notification
        "priority": "HIGH", // Utile pour les notifications
        "type": "REALTIME", // SYSTEM, NOTIFICATION 
        "emitter": "DEBESIUM", // uuid utilisateur
        "source": "assignments",
        "topic": "srv-avenirs.dev.assignments", // our une question de tracabilité
        "operation": "create", // update, delete, utile pour les UID réactives
        "timestamp": "2025-03-05T12:34:56Z",
        "expiration": "2025-03-15T12:34:56Z"
    },
    "payload": { // Contenu variable en fonction des métadata,
        "before": { // Exemple pour debezium
            (...)
        },
        "after": {
            (...)
        }
    },
    "routing": {
       "audience": {
            "type": "USER", // STRUCTURE,FORMATION, etc
            "targets": ["uuid", "uuid", ...]
        },
        "retry": {
              "ack_required": true, // Flag pour l'acquittements
              "enabled": true,
              "attempts": 3,
              "limit": 5,
              "backoff": "EXPONENTIAL"
        }
    }
}
``` 

## Gestion des acquittements
- Tous les messages ne sont pas à acquitter.
- 2 niveaux d'acquittement: le message dans son ensemble pour indiquer qu'il ne doit plus être consommé, l'acquittement pour chaque utilisateur de l'audience.

- Pour les messages devant être acquités, on utilise un enregistrement de type hash dans KeyDB initialisé par le backend. Lorsque le message est acquitté pour tous les membres de l'audience, alors le message dans son ensemble est acquitté.
- Les messages non acquités dans un délai sont rejoués pour les utilisateurs qui n'ont pas l'acquittement.
- Si le nombre de tentatives de retry est atteint, alors le message est placé dans un log d'erreur.

### Mécanisme pour rejouer les messages
Les messages dans KeyDB streams sont immutables, donc il n'est pas possible de modifier uniquement le champ attempts. Deux opérations sont nécessaires:
- Acquitter le message.
- Si le nombre d'attempts est égal à limit alors le messgae est placé dans un log d'erreur.
- Sinon une copie de ce message avec un attempt incrémenté est ajouté à KeyDB par le backend.

**Remarques :** 
- id sont générés par Kafka et propagé dans KeyDB. Les messages rejoués concervent le meme id pour une question de tracabilité. 
- la persistence est à paramétrée au nivau de KeyDB.
- Il faudra probablement un noeud de réplication spécifique au niveau de Postgres pour éviter d'impacter les performances du reste du système.