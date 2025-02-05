---
layout: page
title: Tests de charge
permalink: /load-tests/
up: ../arch/
---


# Tests de charge

## Resultats

### 50 utilisateurs concurents
**Base de données :**
* 50 utilisateurs
* 4836 assignations de rôles

| Type | Name | # Requests | # Fails | Average (ms) | Min (ms) | Max (ms) | Average size (bytes) | RPS | Failures/s |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| POST | /avenirs-portfolio-security/oidc/login | 4826 | 0 | 28.83 | 21 | 322 | 41 | 20.14 | 0 |
| GET | /access-control/authorize | 60323 | 0 | 29.27 | 20 | 258 | 148.25 | 251.74 | 0 |
| GET | /avenirs-portfolio-security/roles | 4826 | 0 | 57.79 | 19 | 369 | 65327.46 | 20.14 | 0 |
|  | Aggregated | 69975 | 0 | 31.21 | 19 | 369 | 4636.1 | 292.02 | 0 |
