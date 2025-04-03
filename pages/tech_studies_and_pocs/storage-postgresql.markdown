---
layout: page
title: PostgreSQL
permalink: /storage-postgresql/
---
## Table of Contents
- [Resources](#resources)
- [Objective](#objective)
  - [Available strategies](#available-strategies)
  - [Key points for the first experiments](#key-points-for-the-first-experiments)
- [Tested strategy](#tested-strategy)
- [Write-Ahead Log Shipping](#write-ahead-log-shipping)
  - [Streaming replication](#streaming-replication)
- [Implementation steps](#implementation-steps)
  - [Primary](#primary)
  - [Secondaries](#secondaries)
- [Docker implementation](#docker-implementation)

## Resources[⇧](#table-of-contents)
- [https://www.postgresql.org/docs/current/](https://www.postgresql.org/docs/current/){:target="_blank"}
- [WAL Introduction](https://www.postgresql.org/docs/current/wal-intro.html){:target="_blank"}
- [https://github.com/zalando/spilo](https://github.com/zalando/spilo){:target="_blank"}
- [https://github.com/zalando/patroni](https://github.com/zalando/patroni){:target="_blank"}
- [https://wiki.postgresql.org/wiki/Replication,_Clustering,_and_Connection_Pooling](https://wiki.postgresql.org/wiki/Replication,_Clustering,_and_Connection_Pooling){:target="_blank"}
- [https://hub.docker.com/postgres](https://hub.docker.com/_/postgres/){:target="_blank"}


## Objective[⇧](#table-of-contents)
The aim is to add, as a starting point in the dev env (docker containers), a High Availability PostgreSQL cluster with replication, load balancing and failover.
This would consist of a Master (primary) for the write operations and 2 secondaries (standby)  with load balancing for the read-only operations.


## Available strategies[⇧](#table-of-contents)
There are several available strategies for different scenarios: [comparison of different solutions](https://www.postgresql.org/docs/current/different-replication-solutions.html){:target="_blank"}


## Key points for the first experiments[⇧](#table-of-contents)
- Synchronous replication (minimal asynchrony).
- Minimal risk of data loss.
- Service level replication: no Shared Disk Failover : material failure or I/O errors would be shared too. File System Replication (see [DRBD](https://fr.wikipedia.org/wiki/DRBD){:target="_blank"}) could be used as a failover solution but not for replication.
- Keep as simple as possible : replication of the entire database, no data partitioning (i.e.: tables split into sets)  and consequently no Parallel Query Execution.
- No risk of data loss: discard Trigger-Based Primary-Standby Replication, as according to the documentation "there is possible data loss during fail over".
- Hot secondaries: the replication needs to be synchronous for the load balancing.
- Transparent: no adaptation of the code or queries to the used strategy. This discard the SQL-Based Replication Middleware: adaptation of function like CURRENT_TIMESTAMP and transactions must be managed for all servers independently.
- No multi masters.

## Tested strategy[⇧](#table-of-contents)

The first tested strategy is Write-Ahead Log Shipping. 
In a second step, Logical Replication (table level replication) and Synchronous Multimaster Replication could be tested. NB: This is not implemented directly in Postgres, it has to be implemented in the app code.
In a third step, we could experiment Data Partitioning and Multiple-Server Parallel Query Execution.

### Write-Ahead Log Shipping
Two implementations: [Log-Shipping](https://www.postgresql.org/docs/current/warm-standby.html) and [Streaming Replication](https://www.postgresql.org/docs/current/warm-standby.html#STREAMING-REPLICATION). The last one will be used as there is less latency.


### Streaming replication
- Principle: [WALL](https://www.postgresql.org/docs/current/wal-intro.html){:target="_blank"} replication
- Small delay (smaller than with log-shipping)
- archive_timeout not required (data loss window)
- **wal_keep_size** this has to be set carefully (large enough): **if WAL segment are recycled too quickly, the standby server has to be reinitialized.**


## Implementation steps[⇧](#table-of-contents) 
### Primary
1. Create a PostgreSQL replication user and allow connections to replication database from secondaries.
2. Creates [replication slots](https://www.postgresql.org/docs/current/warm-standby.html#STREAMING-REPLICATION-SLOTS){:target="_blank"} for each secondary.
3. Activate the [continuous archiving](https://www.postgresql.org/docs/current/continuous-archiving.html){:target="_blank"}. NB.: the folder where the WAL files are copied must not be hosted on the primary.

### Secondaries
1. Restore each secondary from a primary [backup](https://www.postgresql.org/docs/current/continuous-archiving.html#BACKUP-PITR-RECOVERY){:target="_blank"}
2. Create a file called **standby.signal** in the data directory of each secondary.
3. Modify the file postgresql.conf of each secondary to set the connection info to the primary and the copy command (for WAL restoring)


## Docker implementation[⇧](#table-of-contents)
- The archive directory is a docker volume mounted on the primary and secondaries.
- The modified postgresql.conf and pg_hba.conf (Primary & Secondaries - step 3) are mounted via docker.
- The sql initialization of the primary is executed via a docker entry point (Primary - step 1).
- For the secondaries, the database initialization  and the creation of standby.signal is handled in the docker command of the service.

<br/>
[See the implementation in the development environment](https://github.com/avenirs-esr/srv-dev/blob/main/services/postgresql/docker-compose.yml){:target="_blank"}
