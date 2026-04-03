---
layout: page
title: Contrats de communication inter-features
permalink: /microservices-contrats/
page_content_classes: table-container
---


## Stratégie de communication

Ce document liste l'ensemble des communications inter-features du projet. Chaque appel est classifié selon le type de communication le plus adapté au cas d'usage.

### Comparatif REST / gRPC / Kafka

| **Critère**    | **REST**                                   | **gRPC**                                    | **Kafka**                                    |
|----------------|--------------------------------------------|---------------------------------------------|----------------------------------------------|
| Protocole      | HTTP/1.1 + JSON                            | HTTP/2 + Protobuf (binaire)                 | Événements asynchrones                       |
| Latence        | Modérée                                    | Faible (5-10x plus rapide)                  | Variable (ms à sec)                          |
| Couplage       | Synchrone, couplage temporel               | Synchrone, couplage temporel                | Asynchrone, découplé                         |
| Outillage      | Swagger/OpenAPI, curl, navigateur          | Fichiers .proto, génération de clients      | Kafka UI, Schema Registry                    |
| Cas d'usage    | Lectures, validations, CRUD classique      | Haute performance, streaming                | Notifications, side-effects, fan-out         |
| Complexité     | Faible (Spring Boot natif)                 | Moyenne (proto, codegen)                    | Moyenne (broker, topics, consumers)          |

<br/>
**Choix pour ce projet :** REST pour les appels synchrones (l'appelant a besoin de la réponse), Kafka pour les side-effects asynchrones (notifications, initialisations en cascade). gRPC n'est pas retenu car les volumes d'appels ne justifient pas la complexité supplémentaire.

### Règle de décision

- **REST :** l'appelant a besoin du résultat pour continuer (lecture de données, validation, vérification d'existence).
- **Kafka :** l'appelant n'attend pas de réponse, il notifie qu'un fait s'est produit (création, suppression, nettoyage en cascade).

## Matrice des communications inter-features

Le tableau ci-dessous répertorie chaque appel cross-feature, la méthode invoquée, le type de communication retenu et une note explicative.

### AMS

| **Feature source** | **Feature cible**  | **Méthode appelée**                              | **Type** | **Justification**                      |
|--------------------|--------------------|--------------------------------------------------|----------|----------------------------------------|
| ams                | trace              | `getTracesLinkedWithAMS(ams)`                    | REST     | Besoin du count pour l'affichage       |
| ams                | student.imported   | `getSkillLevelProgressesLinkedWithAMS(ams)`      | REST     | Besoin du count pour l'affichage       |

<br/>

### Activity

| **Feature source** | **Feature cible**  | **Méthode appelée**                              | **Type** | **Justification**                      |
|--------------------|--------------------|--------------------------------------------------|----------|----------------------------------------|
| activity           | student.declared   | `getByActivity(activity)`                        | REST     | Vérifier si l'étudiant est inscrit     |
| activity           | student.declared   | `getAllDeclaredActivitiesOf(student)`             | REST     | Lister les inscriptions pour affichage |
| activity           | file               | `getActivityBanner(activity)`                    | REST     | Récupérer l'image bannière             |

<br/>

### Student — Declared Activity

| **Feature source**  | **Feature cible** | **Méthode appelée**                              | **Type** | **Justification**                        |
|---------------------|-------------------|--------------------------------------------------|----------|------------------------------------------|
| student.decl.act    | activity          | `getActivityById(activityId)`                    | REST     | Valider l'existence avant inscription    |
| student.decl.act    | trace             | `findAllTracesById(traceIds)`                    | REST     | Récupérer les traces pour association    |
| student.decl.act    | trace             | `getTracesView(...)`                             | REST     | Recherche paginée pour l'UI              |
| student.decl.act    | association       | `createAll / getAllOf / deleteAllByIds`           | REST     | Gestion des associations trace-activité  |

<br/>

### Student — Declared Skill Progress

| **Feature source**  | **Feature cible** | **Méthode appelée**                              | **Type** | **Justification**                        |
|---------------------|-------------------|--------------------------------------------------|----------|------------------------------------------|
| student.decl.skill  | trace             | `getTracesLinkedWithDeclaredSkillProgress(...)`   | REST     | Afficher les traces liées                |
| student.decl.skill  | trace             | `programNameOfTrace(trace)`                      | REST     | Afficher le nom du programme             |
| student.decl.skill  | trace             | `unassociateTraces(...)`                         | REST     | Désassocier avant suppression            |
| student.decl.skill  | declaredskill     | `getOrCreateFromExternalSkill(...)`              | REST     | Sync compétence externe                  |

<br/>

### Student — Declared Experience

| **Feature source**  | **Feature cible** | **Méthode appelée**                              | **Type** | **Justification**                        |
|---------------------|-------------------|--------------------------------------------------|----------|------------------------------------------|
| student.decl.exp    | user              | `getStudentById(studentId)`                      | REST     | Récupérer le Student pour création       |

<br/>

### Student — Declared Program

| **Feature source**  | **Feature cible** | **Méthode appelée**                              | **Type** | **Justification**                        |
|---------------------|-------------------|--------------------------------------------------|----------|------------------------------------------|
| student.decl.prog   | user              | `getStudentById(studentId)`                      | REST     | Récupérer le Student pour création       |

<br/>

### Student — Imported Progress

| **Feature source**  | **Feature cible** | **Méthode appelée**                              | **Type** | **Justification**                        |
|---------------------|-------------------|--------------------------------------------------|----------|------------------------------------------|
| student.imported    | trace             | `getTracesLinkedWithSkillLevelProgress(...)`     | REST     | Comptage traces par compétence           |

<br/>

### Self-Knowledge

| **Feature source** | **Feature cible** | **Méthode appelée**                              | **Type** | **Justification**                        |
|--------------------|--------------------|--------------------------------------------------|----------|------------------------------------------|
| selfknowledge      | user               | `addSelfKnowledgeCategories(student, cats)`      | REST     | Ajout catégories au student              |
| selfknowledge      | user               | `removeSelfKnowledgeCategory(student, cat)`      | REST     | Retrait catégorie du student             |

<br/>

### Trace

| **Feature source** | **Feature cible**  | **Méthode appelée**                                | **Type** | **Justification**                        |
|--------------------|--------------------|-----------------------------------------------------|----------|------------------------------------------|
| trace              | user               | `getUser(userId)`                                   | REST     | Récupérer le User pour création de trace |
| trace              | student.imported   | `findStudentProgressesBySkillLevelProgresses(...)`  | REST     | Déterminer le nom du programme           |
| trace              | file               | `findByTrace(trace)`                                | REST     | Récupérer les pièces jointes             |
| trace              | student.decl.act   | `findAllDeclaredActivitiesByIds(...)`               | REST     | Récupérer activités pour associations    |
| trace              | student.decl.act   | `findAllNotCompletedActivitiesByIds(...)`           | REST     | Filtrer activités non terminées          |
| trace              | student.decl.act   | `searchDeclaredActivity(keyword, page)`             | REST     | Recherche paginée pour association       |
| trace              | student.decl.skill | `findAllDeclaredSkillProgressesByIds(...)`          | REST     | Récupérer skills pour associations       |
| trace              | student.decl.skill | `searchDeclaredSkill(keyword, page)`                | REST     | Recherche paginée pour association       |
| trace              | association        | `getAllOf / createAll / deleteAllByIds`              | REST     | Gestion des associations                 |

<br/>

### File

| **Feature source** | **Feature cible** | **Méthode appelée**                              | **Type** | **Justification**                        |
|--------------------|-------------------|--------------------------------------------------|----------|------------------------------------------|
| file               | trace             | `getTraceById(traceId)`                          | REST     | Valider l'existence de la trace          |
| file               | trace             | `deleteById(traceId)`                            | REST     | Supprimer trace si upload échoue         |

<br/>

### Program

| **Feature source** | **Feature cible**  | **Méthode appelée**                              | **Type** | **Justification**                        |
|--------------------|--------------------|--------------------------------------------------|----------|------------------------------------------|
| program            | student.imported   | `findAllStudentProgressesByStudent(student)`     | REST     | Config institution via les parcours      |

<br/>

### Shared

| **Feature source** | **Feature cible** | **Méthode appelée**                              | **Type** | **Justification**                        |
|--------------------|-------------------|--------------------------------------------------|----------|------------------------------------------|
| shared             | user              | `getStudentById(userId)`                         | REST     | Récupérer le student connecté            |

<br/>

### User

| **Feature source** | **Feature cible** | **Méthode appelée**                              | **Type** | **Justification**                        |
|--------------------|-------------------|--------------------------------------------------|----------|------------------------------------------|
| user               | file              | `getUserPhotoUrl(...)`                           | REST     | Récupérer les URLs photos                |

<br/>

### Événements asynchrones (Kafka) — candidats

| **Feature source**  | **Feature cible** | **Événement / Méthode**                                          | **Type** | **Justification**                                      |
|---------------------|-------------------|------------------------------------------------------------------|----------|--------------------------------------------------------|
| user                | selfknowledge     | `StudentCreated` → `initSelfKnowledgeCategoriesMandatory`       | Kafka    | Side-effect : init catégories après création étudiant  |
| student.decl.skill  | trace             | `DeclaredSkillDeleted` → `unassociateTraces`                    | Kafka    | Side-effect : nettoyage associations après suppression |

<br/>

## Synthèse

### Récapitulatif

- **30 appels REST synchrones** — l'appelant a besoin de la réponse immédiatement.
- **2 événements Kafka asynchrones** — side-effects de type « fire and forget ».
- **0 appel gRPC** — non retenu, volumes insuffisants pour justifier la complexité.

### Feature la plus couplée : Trace

Le service Trace est le plus interconnecté : il dépend de 6 autres features (user, student.imported, file, student.declared.activity, student.declared.skill, association) et 5 features dépendent de lui. C'est le dernier service à extraire en microservice.

### Ordre d'extraction suggéré

En partant des features les moins couplées vers les plus couplées :

1. **association** — aucune dépendance sortante
2. **selfknowledge** — dépend uniquement de user
3. **program** — dépend uniquement de student.imported
4. **file** — dépend de trace (1 service)
5. **ams** — dépend de trace et student.imported
6. **user** — dépend de file et selfknowledge
7. **student.*** — sous-features declared et imported, dépendent de activity, trace, user
8. **activity** — dépend de student.declared et file
9. **trace** — le plus couplé, extraire en dernier
