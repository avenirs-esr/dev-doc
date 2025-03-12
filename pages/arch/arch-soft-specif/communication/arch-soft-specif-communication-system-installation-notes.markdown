---
layout: page
title: Dossier d'Architecture logicielle - Module Communication - notes d'installation
permalink: /arch-soft-specif-communication-system-installation-notes/
up: ../arch-soft-specif-index/
page_content_classes: table-container
---
[Retour](arch-soft-specif-communication.markdown)<br/>
<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Module Communication - Notifications et temps réel - Notes d'installation.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 05/03/2025<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Notes d'installation correspondant au [plan de mise en oeuvre](../arch-soft-specif-communication-system-integration-plan/){:target="_blank"} dans [l'environnement de développement](../srv-dev/){:target="_blank"}.<br/>
<br/>

## Debezium

### Configuratio de Postgres
Le serveur postgres utilisé est postgresql-secondary1, mais les deux secondaries sont configurés pour autoriser le CDC.

**Base de données :** realtime_db<br/>
**Tables :** public.sample

#### Réplication logique
Mettre en place de la réplication logique au niveau de postgres sur les secondaries.

Fichier de configuration **postgresql.conf** (/avenirs-postgresql-overlay/secondaries_postgresql.conf)

```
wal_level = logical            
max_wal_senders = 10           
max_replication_slots = 10     
```

**Explications :**

- wal_level = logical : niveau de réplication logique qui inclut les métadonnées nécessaires pour le CDC (Change Data Capture).
- max_wal_senders: processus de réplication qui envoient les données au slots. 
- [max_replication_slots](https://www.postgresql.org/docs/current/warm-standby.html#STREAMING-REPLICATION-SLOTS){:target="_blank"} : réservation de conservation des journaux de réplication jusqu'à leur consommation. L'idée est d'avoir un connecteur Debezium par base qui utilse un slot. 

#### Accès à la base de données

L'utilisateur debezium est autorisé à se connecter depuis le conteneur debezium :

```
host    all             debezium        172.16.0.0/12           md`
```

#### Paramétrage

**Primary :**

```
CREATE USER debezium WITH PASSWORD '******' LOGIN REPLICATION;

CREATE DATABASE realtime_db;

\c realtime_db

CREATE TABLE public.sample (
    sdb_id               SERIAL PRIMARY KEY,
    sdb_short_txt        VARCHAR(40) NOT NULL,
    sdb_long_txt         TEXT,
    sdb_creation         TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

GRANT CONNECT ON DATABASE realtime_db TO debezium;
GRANT USAGE ON SCHEMA public TO debezium;
GRANT SELECT ON public.sample TO debezium;
GRANT SELECT ON ALL SEQUENCES IN SCHEMA public TO debezium;

CREATE PUBLICATION realtime_pub FOR TABLE public.sample;
```

**Explicitations :**
- Création de la base de la table du rôle et positionnement des droits.
- Création de la publication. La création doit être faite sur le primary car les secondaries sont en lecture seules.

**Secondaries :**

```

SELECT pg_create_logical_replication_slot('debezium_slot', 'pgoutput');

SELECT * FROM pg_publication;
```

**Explicitations :**
- Création du slot logique de réplication sur lequel le connecteur Debezium va se connecter.

## Création du connecteur postgres

```
curl -X PUT -H "Content-Type: application/json"   --data '{
    "name": "postgres-connector",
    "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
    "database.user": "ebezium",
    "database.password": "********",
    "database.hostname": "postgres",
    "database.port": "5432",
    "database.dbname": "realtime_db",
    "database.server.name": "realtime_db_server",
    "topic.prefix": "realtime_db_rt",
    "slot.name": "debezium_slot",
    "publication.name": "realtime_db_publications",
    "plugin.name": "pgoutput",
    "key.converter": "org.apache.kafka.connect.json.JsonConverter",
    "value.converter": "org.apache.kafka.connect.json.JsonConverter",
    "key.converter.schemas.enable": "false",
    "value.converter.schemas.enable": "false"
  }'   http://localhost:8083/connectors/postgres-connector/config
```

### Vérification

- http://localhost:8083/connectors
```
["postgres-connector"]
``` 

- http://localhost:8083/connectors/postgres-connector/status

```json
{
  "name": "postgres-connector",
  "connector": {
    "state": "RUNNING",
    "worker_id": "172.18.0.23:8083"
  },
  "tasks": [
    {
      "id": 0,
      "state": "RUNNING",
      "worker_id": "172.18.0.23:8083"
    }
  ],
  "type": "source"
}
```
