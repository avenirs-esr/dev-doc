---
layout: page
title: Architecture - Authentication - Initial implementation
permalink: /initial-implementation/
up: ../authentication/

---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Mise en œuvre d'une première authentification locale basée sur OIDC.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 04/01/2026<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Table des matières

- [Objectifs](#objectifs)
- [Première mise en œuvre simplifiée](#première-mise-en-œuvre-simplifiée)
  - [Simplifications](#simplifications)
  - [Diagramme de séquence](#diagramme-de-séquence)
- [Données nécessaires pour les différents microservices](#données-nécessaires-pour-les-différents-microservices)
  - [Microservices impliqués](#microservices-impliqués)
  - [Modélisation actuelle dans avenirs-portfolio-api](#modélisation-actuelle-dans-avenirs-portfolio-api)
- [Refactoring du modèle de données](#refactoring-du-modèle-de-données)
  - [Microservice d'interopérabilité : avenirs-portfolio-interoperability](#microservice-dinteropérabilité--avenirs-portfolio-interoperability)
  - [Microservice de sécurité : avenirs-portfolio-security](#microservice-de-sécurité--avenirs-portfolio-security)
  - [Microservice métier du portfolio : avenirs-portfolio-api](#microservice-métier-du-portfolio--avenirs-portfolio-api)
- [Architecture possible pour le processus d'alimentation (informatif, non arrêté)](#architecture-possible-pour-le-processus-dalimentation-informatif-non-arrêté)
  - [Schéma du processus d'alimentation](#schéma-du-processus-dalimentation)
- [Intégration avec l'API Manager](#intégration-avec-lapi-manager)
  - [Principe](#principe)
  - [Bénéfices de la démarche](#bénéfices-de-la-démarche)
  - [Remarques](#remarques)
  - [Diagramme de séquence](#diagramme-de-séquence-2)
- [Adaptations du filtre de sécurité au payload transmis par l'API Manager](#adaptations-du-filtre-de-sécurité-au-payload-transmis-par-lapi-manager)
- [Evolutions](#evolutions)

<br/>

## Objectifs[⇧](#table-des-matières)
- Mise en place d'une authentification locale basée sur OIDC de bout en bout faisant intervenir :
   - Le front (client).
   - CAS en tant qu'OIDC provider : authentifie et fournit un access token.
   - L'API Manager : interagit avec le microservice de sécurité afin de vérifier le token et de fournir un json signé représentant l'utilisateur à l'ensemble des microservices.
   - Le microservice avenirs-portfolio-api : pour vérifier que les données transmises par l'API Manager sont suffisantes.
- Aligner les modèles de données utilisateurs entre les microservices.

- Simuler l'alimentation pour propager les données entre le microservice d’interopérabilité, le LDAP et les autres microservices.
- Réfléchir à la gestion multi-comptes / multi-établissements.

La démarche adoptée est de commencer par une version minimaliste, puis d'améliorer par raffinements successifs.

## Première mise en œuvre simplifiée[⇧](#table-des-matières)

### Simplifications

- On considère uniquement l'obtention initiale de l'access token.
- Le client réalise directement la demande d'authentification auprès de CAS, l'étape de landing page est ignorée pour cette première mise en œuvre.
- On utilise directement l'access token fourni par l'utilisateur.

### Diagramme de séquence

{% include img.html
        src="assets/images/authentication_first_step.png"
        alt="Authentification implémentation initiale"
        caption="Obtention initiale d'un access token et utilisation pour les microservices"
        width="60%"
    %}


## Données nécessaires pour les différents microservices[⇧](#table-des-matières)

### Microservices impliqués

Trois microservices sont impliqués :
 - avenirs-portfolio-interoperability: récupération, après consolidation des données issues des SI des établissements.
 - avenirs-portfolio-security: données utilisateurs nécessaires pour la sécurité.
 - avenirs-portfolio-api: API métier du portfolio.

### Modélisation actuelle dans avenirs-portfolio-api

{% include img.html 
 src="assets/images/users_in_api_initial_state.png" 
 alt="Modèle actuel" caption="Modèle actuel : tout est dans avenirs-portfolio-api et external_user porte le lien vers user" 
 width="800px"
 %}

<br/>
**Modifications à apporter :**
- Modéliser l'objet de sécurité **principal** avenirs-portfolio-security (principal représente un utilisateur d'un point de vue sécurité). 
- Déplacer la table **external_user** dans **avenirs-portfolio-interoperability.**
- Extraire le lien vers user porté par external_user vers user et la modéliser dans avenirs-portfolio-api entre **user** d'avenirs_portfolio_api et **principal** d'avenirs_portfolio_security.


**Identifiant unique :**
- Relations entre les objets des microservices : Le lien entre les objets de microservices ne peut pas être basé sur l'id (uuid) car il ne permet pas de décorréler les cycles de vie des microservices. Une réalimentation des données d'un des microservices provoque un changement des id et, si les relations entre objets de microservices sont basées sur les id, elles peuvent devenir orphelines.

- Le choix de l'identifiant est à déterminer : eppn, external_id + source, INE pour les étudiants.<br/>
**Pour cette première mise en œuvre on utilisera l'eppn.** <br/>
L'avantage de cet eppn est qu'il garantit une unicité globale, il est constitué de la concaténation identifiant unique utilisateur + identifiant unique établissement. Exemple : uid@domain-etablissement.fr


## Refactoring du modèle de données[⇧](#table-des-matières)

#### Microservice d'interopérabilité : avenirs-portfolio-interoperability.

{% include img.html 
   src="assets/images/external_user.png" 
   alt="External user" 
   caption="Modélisation des external_user dans avenirs-portfolio-interoperability" %}
<br/>

#### Microservice de sécurité : avenirs-portfolio-security

<div style="display: flex; gap: 130px;">
  <div>{% include img.html src="assets/images/principal.png" 
  alt="Principal" caption="Modélisation de principal, l'objet de sécurité utilisateur" %}</div>
  <div>{% include img.html 
   src="assets/images/principal_link.png" alt="Principal link" 
   caption="Une des pistes exploratoires de modélisation pour la gestion multi comptes" %}</div>
</div>
<br/>

**Remarques concernant la liaison entre Objets Principal (ébauche):**
- Le contrôle d'accès serait effectué du principal associé à l'eppn (ou autre id) ainsi que tous les eppn des comptes associés du même type.
- Il faut définir comment sont déterminées les informations utilisateurs retournées pour un compte lié.
- Il faut éviter les cycles au niveau des liaisons.
- Une solution pour adresser ces problématiques pourrait être de limiter les possibilités de liaisons :
   - Prendre en compte le sens de la relation pour déterminer le compte source et les comptes liés, et définir une stratégie de gestion des comptes liés : fusion, notion de sous-compte, etc.
   - Un seul niveau de liaison : un compte lié ne peut à son tour être lié à un autre compte.
   - Limiter le nombre de liaisons possibles pour un compte donné.
   - Prendre en compte le fait que les comptes liés peuvent être de différents types : étudiant, enseignant, etc. ou, au contraire, ne pas permettre de lier des comptes de différents types.

#### Microservice métier du portfolio : avenirs-portfolio-api

{% include img.html 
   src="assets/images/student_teacher_user.png" 
   alt="User Student Teacher" 
   caption="Modélisation des users dans avenirs-portfolio-api" %}
<br/>

## Architecture possible pour le processus d'alimentation (informatif, non arrêté)[⇧](#table-des-matières)

**Remarque :** le processus d'alimentation n'a pas été complètement implémenté, la réflexion est toujours en cours.

### Schéma du processus d'alimentation
{% include img.html src="assets/images/auth_init_provisioning.svg" alt="Processus d'alimentation" caption="Architecture possible pour le processus d'alimentation" %}

<br/>

## Intégration avec l'API Manager[⇧](#table-des-matières)

### Principe
On utilise un plugin Apisix de type serverless en LUA qui va :
- extraire l'access token du header de la requête.
- interroger le microservice de security pour valider l'access token.
- si l'access token est valide, le microservice transmet également les informations relatives au principal (eppn, category, etc).
- le plugin construit un payload qu'il signe et transmet au microservice associé à la route.
- au niveau du microservice un filtre de sécurité spring extrait et valide le payload puis le fourni au microservice (par exemple via SecurityContextHolder.getContext().setAuthentication(...)).

### Bénéfices de la démarche
- Centraliser les opérations de sécurisation des routes et de l'accès aux ressources au niveau de l'API Manager.
- Découpler les microservices.
- Garantir que la requête traitée par les microservices provient de l'API Manager.

### Remarques
Il s'agit d'une simplification pour tester l'authentification. Pour le contrôle d'accès, le plugin devra également extraire :
 - La méthode HTTP et le end-point pour déterminer l'action utilisateur.
 - La ressource cible.
 - Possiblement d'autres informations comme l'univers actif.

### Diagramme de séquence
{% include img.html src="assets/images/apim_auth_first_steps.png" alt="Intégration avec l'API Manager" caption="Intégration avec l'API Manager" %}


## Adaptations du filtre de sécurité au payload transmis par l'API Manager[⇧](#table-des-matières)

Le filtre de sécurité est mutualisé [dans la librairie common](https://github.com/avenirs-esr/avenirs-portfolio-common/blob/219dd0a2b59cb13290bbd68963e87f8785669c57/src/main/java/fr/avenirsesr/portfolio/common/security/infrastructure/filter/DevAuthenticationFilter.java){:target="_blank"}.

Les adaptations devraient être relativement simples :
- Adapter [UserSecurityPayload](https://github.com/avenirs-esr/avenirs-portfolio-common/blob/219dd0a2b59cb13290bbd68963e87f8785669c57/src/main/java/fr/avenirsesr/portfolio/common/security/infrastructure/adapter/model/UserSecurityPayload.java){:target="_blank"} pour l'aligner sur le payload transmis par l'API Manager : eppn, category, etc.
- Adapter le [filtre de sécurité](https://github.com/avenirs-esr/avenirs-portfolio-common/blob/219dd0a2b59cb13290bbd68963e87f8785669c57/src/main/java/fr/avenirsesr/portfolio/common/security/infrastructure/filter/HmacAuthenticationFilter.java#L62){:target="_blank"} pour transmettre le payload au microservice.

## Evolutions[⇧](#table-des-matières)

- Ajouter la landing page d'authentification.
- Gestion du mode non connecté côté front.
- Déterminer s'il faut gérer un access token spécifique au portfolio ou s'il vaut mieux utiliser directement celui du provider OIDC.
- Gestion du logout, pour un seul terminal ou pour tous les terminaux.
- Gestion du refresh.
- Ajouter d'autres mécanismes d'authentification :
   - Fédération ESR.
   - France connect +
   - ...
- Tracer toutes les opérations liées à l'authentification et au contrôle d'accès.
