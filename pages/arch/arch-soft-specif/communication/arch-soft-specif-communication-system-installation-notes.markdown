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
Debezium est une plateform de Capture Data Change (CDC) que l'on utilise dans ce contexte pour capturer les modifications dans la base de données et les transmettre à Kafka.

### Configuration de Postgres

#### Détail du cluster Postgres


{% include img.html
        src="assets/images/rt-cluster-prostgres.svg"
        alt="Détail du cluster Postgres"
        caption="Détail du cluster Postgres"
%}
<br/>

Après essais, les secondaries 1 et 2 ne peuvent pas être utilisés car c'est le mécanisme de réplication physique qui est utilisé et ils sont en mode recovery ce qui est incompatible avec la connexion d'un CDC. (Erreur de type : WAL control functions cannot be executed during recovery.)

Un troisième noeud qui utilise la réplication logique est mis en place : avenirs-pgslq-secondary-cdc.

Le fait d'introduire un troisième noeud et de mettre en place un connecteur Debezium sur ce noeud plutôt que sur le primary permet d'éviter de charger le primary. La charge induite par Debezium est plus importante que la charge de réplication vers le noeud dédié au CDC.

**Base de données :** realtime_db<br/>
**Tables :** public.sample

#### Réplication logique
La réplication logique est utilisée au niveau de primary conjointement avec la réplication physique.


Sur le secondaire dédié au CDC, la réplication logique est également activée. Elle sera utilisée par Debezium.

Fichier de configuration **postgresql.conf** (/avenirs-postgresql-overlay/secondaries_cdc_postgresql.conf)

```
wal_level = logical            
max_wal_senders = 10           
max_replication_slots = 10    
hot_standby = on 
```

**Explications :**

- wal_level = logical : niveau de réplication logique qui inclut les métadonnées nécessaires pour le CDC (Change Data Capture).
- max_wal_senders: processus de réplication qui envoient les données au slots. 
- [max_replication_slots](https://www.postgresql.org/docs/current/warm-standby.html#STREAMING-REPLICATION-SLOTS){:target="_blank"} : réservation de conservation des journaux de réplication jusqu'à leur consommation. L'idée est d'avoir un connecteur Debezium par base qui utilse un slot. 
- hot_standby = on : les requêtes en lecture seule sont autorisées en lecture si le serveur en mode recovery (pas vraiment d'impact dans notre cas). 

#### Accès à la base de données

L'utilisateur debezium est autorisé à se connecter depuis le conteneur debezium :

```
host    all             debezium        172.16.0.0/12           md`
```

#### Paramétrage

**Primary :**

CREATE DATABASE realtime_db;

\c realtime_db

CREATE TABLE public.sample (
    sdb_id SERIAL PRIMARY KEY,
    sdb_short_txt VARCHAR(40) NOT NULL,
    sdb_long_txt TEXT,
    sdb_creation TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
GRANT SELECT ON TABLE public.sample TO pgrepl;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO pgrepl;

CREATE PUBLICATION cdc_pub FOR ALL TABLES;
```

**Explicitations :**
- Création de la base de la table du rôle et positionnement des droits pour l'utilisateur de réplication.
- Création de la publication utilisée par le secondaire pour se synchroniser.

**Secondaire (avenirs-pgsql-secondary-cdc) :**

```

CREATE USER debezium WITH PASSWORD 'p@@ss4DBZCDC' LOGIN REPLICATION;

CREATE DATABASE realtime_db;

ALTER DATABASE realtime_db OWNER TO debezium;
\c realtime_db

GRANT ALL PRIVILEGES ON DATABASE realtime_db TO debezium;
GRANT USAGE ON SCHEMA public TO debezium;
GRANT ALL PRIVILEGES ON SCHEMA public TO debezium;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO debezium;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO debezium;
GRANT ALL PRIVILEGES ON ALL FUNCTIONS IN SCHEMA public TO debezium;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL PRIVILEGES ON TABLES TO debezium;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL PRIVILEGES ON SEQUENCES TO debezium;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL PRIVILEGES ON FUNCTIONS TO debezium;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL PRIVILEGES ON TYPES TO debezium;


CREATE TABLE public.sample (
    sdb_id SERIAL PRIMARY KEY,
    sdb_short_txt VARCHAR(40) NOT NULL,
    sdb_long_txt TEXT,
    sdb_creation TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE SUBSCRIPTION cdc_sub
CONNECTION 'host=avenirs-pgsql-primary dbname=realtime_db user=pgrepl password=pgreplpassword'
PUBLICATION cdc_pub
WITH (copy_data = true);

CREATE PUBLICATION realtime_pub FOR ALL TABLES;

GRANT PUBLICATION realtime_pub TO debezium;

```

**Explicitations :**
- Création de la table, la replication logique ne conserrne que les données (pas de synchronisation DDL).
- Positionnement des droits pour l'utilisateur debezium et positionnement des droits par défaut, en cas d'ajout de tables éventuel.
- Création de la souscription à la publication du primaire.
- Création de la publication utilisée par Debezium.

## Création du connecteur postgres

```
curl -X PUT -H "Content-Type: application/json" \
  --data '{
    "name": "postgres-connector",
    "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
    "database.user": "debezium",
    "database.password": "p@@ss4DBZCDC",
    "database.hostname": "avenirs-pgsql-secondary-cdc",
    "database.port": "5432",
    "database.dbname": "realtime_db",
    "database.server.name": "avenirs_realtime",
    "topic.prefix": "avenirs_rt",
    "slot.name": "debezium_slot",
    "publication.name": "realtime_pub",
    "plugin.name": "pgoutput",
    "key.converter": "org.apache.kafka.connect.json.JsonConverter",
    "value.converter": "org.apache.kafka.connect.json.JsonConverter",
    "key.converter.schemas.enable": "false",
    "value.converter.schemas.enable": "false"
  }' \
  http://localhost:8083/connectors/postgres-connector/config
```

### Vérifications

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

## Insertion d'une ligne dans la table de test

``` sql
 docker exec -it avenirs-pgsql-primary psql -p 5432 -U pguser -d realtime_db -c "INSERT INTO public.sample (sdb_short_txt, sdb_long_txt) VALUES ('Test entry', 'This is a test entry for Debezium CDC testing');"
```

**Logs du conteneur debezium :**
```
2025-03-13 09:08:52,665 INFO   ||  1 records sent during previous 00:02:19.558, last recorded offset of {server=avenirs_rt} partition is {transaction_id=null, lsn_proc=30642856, messageType=INSERT, lsn_commit=30642536, lsn=30642856, txId=760, ts_usec=1741856932053323}   [io.debezium.connector.common.BaseSourceTask]
```

**Vérification des données dans Kafka :**

le topic utilisé par Debezium est "topic.prefix".schema.table (avenirs_rt.public.sample).

{% include img.html
        src="assets/images/rt-check-kafka.png"
        alt="Vérification des données dans Kafka"
        caption="Vérification des données dans Kafka"
%}