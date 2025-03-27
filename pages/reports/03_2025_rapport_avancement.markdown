---
layout: page
title: Avenirs ePortfolio - Rapport d'avancement - 03/2025
is_menu_entry: false
---
# Table des matières

- [Introduction](#introduction)
- [Sécurité et confidentialité](#sécurité-et-confidentialité)
- [État d'avancement du frontend](#état-davancement-du-frontend)
- [État d'avancement du backend](#état-davancement-du-backend)
  - [Architecture](#architecture)
  - [Environnement de développement](#environnement-de-développement)
  - [Tests de charge et validation technique](#tests-de-charge-et-validation-technique)
  - [Avancement par grand module](#avancement-par-grand-module)
    - [API Manager](#api-manager)
    - [Authentification et contrôle daccès](#authentification-et-contrôle-daccès)
    - [Communication](#communication)
    - [Stockage](#stockage)
    - [Monitoring](#monitoring)
    - [Back office](#back-office)
    - [Interopérabilité et intégration dans l'écosystème](#interopérabilité-et-intégration-dans-lécosystème)
- [Conclusion](#conclusion)


# Introduction
Ce rapport d'avancement vise à fournir un aperçu complet de l'état actuel du projet ePortfolio.Il a pour objectif de donner une vision claire et honnête de la trajectoire engagée, des stratégies mises en œuvre, mais aussi des difficultés rencontrées.

Le projet peut être abordé selon de nombreuses dimensions. Certains aspects essentiels, comme la conservation et l'archivage, ou encore la prise en compte des contraintes légales et réglementaires, ne seront pas traités ici, car ils sortent du périmètre de ce rapport d'avancement.

Ce document se concentre sur la partie **technique et réalisation,** suivant quatre grands axes :

- **La sécurité et la confidentialité :** garantir la sécurité et la confidentialité des données utilisateurs ; détecter et résoudre les problèmes ou anomalies.

- **Le frontend :** interfaces utilisateurs, scénarios d'usage et expérience utilisateur. Une spécification complète est un prérequis avant tout développement, car cette partie est très dépendante du domaine métier. L'équipe technique, n'étant pas détentrice de cette expertise, n'est pas autonome sur cette phase. L'accessibilité et l'internationalisation sont des composantes essentielles.

- **Le backend :** les services sur lesquels reposent les interfaces utilisateurs (authentification, contrôle d'accès, stockage, etc.). Cette partie est plus technique et dépend davantage du mode de distribution et de l'envergure du projet. L'équipe est plus autonome pour concevoir l'architecture, réaliser des tests, établir des priorités et commencer à intégrer les services.

- **L'interopérabilité :** définir comment la plateforme pourra s'intégrer dans l'écosystème des établissements.


## Sécurité et confidentialité
Ce projet d'ePortfolio est particulièrement concerné par les problématiques de sécurité. En effet, il est conçu pour être hébergé en mode SaaS/hybride, avec une logique multi-établissements, et concernera une large communauté d'utilisateurs aux profils hétérogènes. Il sera, en outre, interconnecté avec de nombreux services extérieurs.

Pour répondre à ces enjeux, une stratégie basée sur une démarche d'amélioration continue a été retenue : **"Security/Privacy by design"**.

Cette stratégie vise à intégrer les questions de sécurité et de confidentialité dès les premières phases du projet. Les bénéfices sont multiples :

- meilleure efficacité ;
- réduction des coûts ;
- meilleure réactivité en cas de problème : détection, compréhension, remédiation.

La démarche est initiée au niveau du projet, mais doit maintenant être reprise et affinée progressivement. Elle s'appuie sur un ensemble de documents qui formalisent les principes retenus, et qui ont vocation à être déclinés en mesures concrètes : recommandations, outillages et bonnes pratiques.

[Documentation de la démarche](../../security-by-design/){:target="_blank"}

# État d'avancement du frontend

Les interfaces utilisateurs sont la partie émergée de la plateforme. Il est essentiel de produire des écrans qui répondent à un certain nombre de caractérisqiques :
 - accessibles, pour que les utilisateurs en situation de handicape ne soient pas pénalisés,  
 - ergonomiques, une expérience utilsateur réussie est l'un des enjeux centraux de ce projet,
 - traduites (internationnalisation), la communauté universitaire est cosmopolite,
 - responsive, pour que les écrans s'adaptent au dispositifs d'affichage. 
 
 Les parcours utilisateurs doivent être bien pensés, et l'ensemble de ces caractéristiques doit favoriser l'adoption de l'approche par par compétences et l'engagement utilisateur.

Les spécifications frontend ayant pris du retard, nous avons fait appel à une prestation de service sous forme de maîtrise d'œuvre. C'est la société MC2I qui a été retenue pour nous accompagner dans cette phase.
Elle produit les livrables sous forme de récits utilisateurs (User Stories), de parcours utilisateur (User Flows) et de maquettes.
De nombreuses personnes, aux profils variés, sont impliquées via :
- des ateliers de raffinement hebdomadaires en distanciel ;
- des ateliers en présentiel, plus ciblés, une fois par mois.
Malgré l'aide efficace de MC2I, ce processus prend du temps afin aboutir à une solution avec le niveau de maturité attendu pour un projet de cette envergure.

Le développement frontend dépend directement des spécifications, qui ont mis du temps à être lancées. Cette partie reste donc logiquement en retrait par rapport au backend.

D'un point de vue technique, le framework retenu pour les développements est Vue.js. Il fait partie des frameworks les plus utilisés, et il est largement répandu dans la communauté universitaire. Sa courbe d'apprentissage est plus douce que celle de React ou Angular, ses concurrents, ce qui devrait faciliter les collaborations.
L'utilisation de Web Components, permettant de réutiliser des éléments d'interface quel que soit le framework utilisé, est également envisagée. Une petite expérimentation a été menée avec la librairie Lit pour valider cette approche.

Un travail de réflexion plus général a été engagé sur l'organisation des développements frontend. Plusieurs thématiques ont déjà été identifiées et pourront être traitées prochainement pour poser un cadre de travail et une architecture cohérente.
D'autres orientations sont déjà posées, comme :
- l'utilisation d'un store d'état ;
- l'adoption d'une bibliothèque spécialisée pour la gestion des requêtes (data fetching), en lien avec les problématiques du module Communication.
- la nécessité d'avoir un mécanisme de paramétrage centralisé en lien avec le module backoffice.

Les travaux engagés jusqu'à présent ont été menés à effectif très réduit. Les bases sont posées et permettront d'accélérer dès que les livrables de spécification seront livrés et l'équipe renforcée.

[TODO] Montrer des captures d'écran

# État d'avancement du backend

## Architecture
L'objectif est de mettre en place une plateforme SaaS capable d'accueillir jusqu'à 600 000 utilisateurs à court terme (dont environ 300 000 issus des établissements partenaires). Les contraintes de performance et de mise à l'échelle ont orienté l'architecture vers un modèle microservices. Cette approche repose sur un découpage du système en grands domaines fonctionnels, chacun étant totalement autonome et découplé des autres. Elle permet une mise à l'échelle horizontale par duplication des services en fonction de la charge et des usages. Elle facilite également la maintenance et les mises à jour en permettant une gestion et des déploiements indépendants.

{% include img.html
        src="assets/images/architecture-simple-view.svg"
        alt="Modules and their interactions"
        width="50%"
        caption="Main modules"
%}

**Focus sur l'API Manager :** dans cette architecture, une brique particulière, l'API Manager, constitue le centre névralgique de toutes les interactions avec le système. Ce type de solution commence à être adopté par certains établissements en pointe. Une étude préliminaire a conforté la pertinence de son intégration au cœur du système. Ses fonctionnalités et avantages seront développés plus en détail dans la suite de ce document.

## Environnement de développement

Afin de permettre le développement, les tests et les validations techniques, une architecture de développement a été mise en place sous forme de conteneurs Docker. Chaque service de chaque module dispose de son propre conteneur, ce qui permet de transposer fidèlement l'architecture microservices et de reproduire au plus près le futur environnement de production. Cet environnement est versionné dans un dépôt GitHub et peut être déployé très simplement, aussi bien localement par les développeurs que sur un serveur de test. Il évolue en fonction de l'avancée des travaux.

{% include img.html
        src="assets/images/docker-containers.svg"
        alt="Dockers containers"
        width="80%"
        caption="Schéma d'intégration des services et des développements dans l'environnement de développement"
    %}

<br/><br>
[Dépôt GitHub (branche d'intégration temps réel)](https://github.com/avenirs-esr/srv-dev/tree/feature/realtime-integration){:target="_blank"}

## Tests de charge et validation technique

Une stratégie de tests de charge a été initiée afin d'obtenir des premières métriques, de valider certains choix techniques et de lever des interrogations. Elle repose sur la génération de données de test en volumes croissants, croisée avec la simulation d'un nombre également croissant d'interactions utilisateurs simultanées.
Ces tests sont réalisés sur l'environnement de développement, avec pour objectif d'être reproduits ensuite sur l'environnement de production. Ils ont été présentés et validés par l'équipe en charge de l'hébergement. Cette démarche permettra d'obtenir une vision claire des performances et des limites du système, ainsi que de réaliser les éventuels ajustements ou dimensionnements nécessaires.


- [Documentation sur la méthodologie de tests de charge](../../load-tests/){:target="_blank"}
- [Exemple de rapport de tests de charge](../../static-pages/load-tests/reports/m1.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}
## Avancement par grand module

Cette section présente l'état des principaux modules du backend. Certains sont déjà bien avancés, d'autres sont encore en phase de réflexion ou à initier.
La priorité a été donnée aux modules que l'on pensait nécessaires pour commencer le développement de la partie frontend, comme l'authentification et le contrôle d'accès, ou encore le module de communication, pour les notifications et la mise en place d'interfaces utilisateur réactives (mise à jour dynamique des contenus).

### API Manager
Ce type de produit offre un ensemble de fonctionnalités particulièrement intéressantes dans notre contexte, parmi lesquelles :

- une gestion unifiée des API et de leurs routes ;
- la possibilité de fixer et gérer des limites d'utilisation ;
- une bonne intégration avec différents protocoles, notamment les protocoles d'authentification ;
- des fonctionnalités avancées autour des problématiques de sécurité et de l'observabilité ;
- des facilités en matière de maintenance et d'évolution, avec par exemple, la possibilité de mettre en place des canary releases, qui consiste à déployer une nouvelle version ou fonctionnalité à un groupe restreint d'utilisateurs.

C'est pourquoi les premiers travaux menés ont porté sur l'API Manager, qui est devenu une brique centrale de l'architecture mise en place.
Ils se sont déroulés en plusieurs phases, en commençant par des échanges avec des collègues de l'Université de Lille, déjà engagés dans un déploiement similaire, ainsi qu'avec certains fournisseurs de solutions.

Une phase d'analyse et la mise en place de plusieurs POC (Proof of Concept) nous ont permis de monter en compétences sur le sujet et d'affiner notre vision.
Les solutions de type SaaS ont été écartées, et notre choix, parmi les différentes options restantes, s'est porté sur Apache APISIX, un produit de la fondation Apache.
Outre ses possibilités techniques, d'autres considérations ont été prises en compte, notamment le type de licence et la politique de tarification.

Notre stratégie sur cette brique est de rester dans une utilisation la plus standard possible, afin de limiter l'adhérence à un produit particulier.


### Authentification et contrôle d'accès

Pour ce module, la question s'est posée de réaliser un développement spécifique ou de s'appuyer sur un service existant de type IAM (Identity and Access Management). Ces solutions nous sont apparues comme trop complexes, tant sur le plan fonctionnel que sur celui de leur mise en œuvre.
Nous souhaitions également conserver une maîtrise complète de ce module, à la fois pour des raisons de sécurité et de performances, puisqu'il intervient à chaque requête utilisateur.

La conception s'est appuyée sur un modèle classique de type RBAC (Role-Based Access Control). Le développement a été réalisé, et s'interface avec un système d'authentification CAS, très répandu dans les établissements. Il est utilisé ici comme fournisseur OpenID Connect (OIDC).
Des ajustements seront probablement nécessaires pour intégrer d'autres protocoles d'authentification et s'adapter aux besoins qui émergeront lors du développement du frontend, mais l'essentiel est d'ores et déjà finalisé.

Ce développement a aussi été l'occasion de poser les bases d'une chaîne d'intégration continue (CI). Celle-ci exécute des tests unitaires, des tests de charge, ainsi que des mesures de couverture, dont les rapports sont publiés sur le dépôt GitHub. Les différents éléments constituant cette chaîne ont été conçus de façon à pouvoir être réutilisés pour les autres modules du projet.
Ce travail a permis de définir un mode opératoire reproductible, qui servira de base à la mise en place des chaînes d'intégration futures, incluant notamment l'incorporation de tests de sécurité.

{% include img.html
        src="assets/images/architecture-security-rbac.svg"
        alt="RBAC Security model"
        width="70%"
        caption="Schéma du mécanisme d’évaluation des droits basé sur le modèle RBAC"
    %}
    
<br/>


[Documentation du module de contrôle d'accès](../../arch-soft-specif-security-rbac/){:target="_blank"}

### Communication
Ce module couvre les différents canaux de communication mis à disposition des utilisateurs de la plateforme. Il englobe à la fois la communication entre utilisateurs et celle entre le backend et le frontend.
Pour la communication entre utilisateurs, il peut s'agir de l'envoi de mails, de messages, de notifications ou encore d'un système de chat.
La communication entre le backend et le frontend concerne notamment la gestion des droits, comme l'ouverture ou la révocation d'autorisations. Il s'agit également des fondations techniques permettant de construire des interfaces réactives, dont les éléments se mettent à jour automatiquement sans rechargement de la page.

Ce module est très structurant et étroitement lié au développement du frontend. C'est pourquoi il a été priorisé, juste après le développement du module en charge de l'authentification et du contrôle d'accès.

L'objectif global de ce sous-système est de fluidifier l'utilisation de la plateforme en offrant une expérience utilisateur agréable et riche, afin de favoriser l'engagement.

Une étude technique a été menée et plusieurs expérimentations ont été réalisées sur cette thématique. Cela a permis de définir une architecture et de faire les choix techniques nécessaires, formalisés dans un plan de mise en œuvre.
Le développement est en cours ; l'implémentation dans l'environnement de développement est réalisée parallèlement, dans une branche spécifique du dépôt.

Dans une première version, on se limite aux notifications simples, sans prise en charge des notifications push (qui fonctionnent même lorsque l'application est fermée).
La partie mail sera également traitée dans un second temps, car elle relève davantage de l'ingénierie système et n'impacte pas le développement frontend.

Nous avons encore quelques interrogations concernant les ressources nécessaires. Ces éléments ont été intégrés dans le plan de mise en œuvre, avec des pistes d'optimisation et des mécanismes de désactivation progressive (fallback) de tout ou partie des fonctionnalités si besoin.

Le diagramme et les liens ci-dessous présentent l'architecture et les travaux réalisés sur le sujet.

{% include img.html
        src="assets/images/arch-soft-specif-communication-system-integration-plan.svg"
        alt="Schéma d'architecture du système temps réel"
        width="50%"
        caption="Architecture d'intégration du système de communication temps réel."    
%}
    
<br/>

- [Etudes préliminaires.](../../arch-soft-specif-communication-preliminary-study/){:target="_blank"}
- [Plan de mise en œuvre technique.](../../arch-soft-specif-communication-system-integration-plan/){:target="_blank"}


Les trois modules suivants ont été identifiés comme moins prioritaires dans cette première phase. Leur mise en œuvre est encore en cours de cadrage. Les travaux sur ces thématiques sont prévus et seront menés avec la même stratégie que ceux déjà engagés. Des recrutements sont en cours pour renforcer l'équipe, ce qui devrait permettre de maintenir une bonne trajectoire dans la suite du projet. 


### Stockage
Différents types de stockage seront utilisés : bases de données relationnelles et non relationnelles, ainsi que des caches en mémoire.

Le besoin d'un stockage versionné a été identifié, notamment pour la gestion des traces. Il est considéré comme prioritaire, car nécessaire au frontend pour certains scénarios d'utilisation, comme les évaluations ou les demandes de feedback.
L'évaluation de son impact en termes de volume de stockage et de performances est un travail à mener.
À ce stade, la solution pressentie repose sur un stockage de type S3, par exemple avec MinIO. Toutefois, ce type de solution stocke chaque version dans son intégralité, sans gestion des différentiels. Il est donc nécessaire de prévoir un mécanisme plus bas niveau, système de fichiers ou baie de stockage, pour implémenter cette fonctionnalité sans explosion du coût de stockage. L'équipe en charge de l'hébergement pourra nous aider sur ce point.

Certains travaux ont déjà été engagés, notamment la mise en place d'un cluster PostgreSQL pour les besoins de stockage relationnel. Des études ont été menées sur différents modes de réplication, et une première implémentation a été réalisée dans l'environnement de développement. Ces éléments devront être approfondis en lien avec l'équipe hébergement, notamment pour ce qui est des questions de performance et de haute disponibilité.

Le choix qui a été fait est de déployer un seul cluster PostgreSQL au sein duquel chaque microservice dispose de sa propre base de données. Ce choix permet de garantir l’isolation logique entre les services tout en mutualisant l’administration et les ressources du cluster.

De même, une base en mémoire de type KeyDB, déjà utilisée dans l'architecture du module de communication, pourra jouer un rôle dans la gestion des caches à différents niveaux. Là encore, il s'agit d'une phase d'optimisation qui pourra intervenir dans un second temps.

{% include img.html
        src="assets/images/rt-cluster-prostgres.svg"
        alt="Cluster PostgreSQL mis en œuvre pour la réplication physique et logique"
        width="50%"
        caption="Cluster PostgreSQL mis en œuvre pour la réplication le Capture Data Change (CDC)."    
%}
<br/>


### Monitoring

Le monitoring est essentiel, mais concerne principalement la phase d'exploitation. Il interviendra donc plus tard dans la vie du projet. Plusieurs dimensions devront être prises en compte :

- L'observabilité, qui permettra de suivre le fonctionnement du système et de détecter au plus tôt d'éventuels dysfonctionnements. L'API Manager joue un rôle central à ce niveau dans une architecture microservices, ce qui conforte le choix de son intégration. L'objectif est de maintenir le système dans un état nominal afin d'assurer une qualité de service satisfaisante pour les utilisateurs.
 L'analyse des usages, qui vise à comprendre comment la plateforme est utilisée. L'objectif est à la fois de rendre compte de l'utilisation effective, mais aussi de disposer d'éléments pour évaluer la pertinence des fonctionnalités en place ou identifier des évolutions à envisager. Différents types d'analyses seront à mettre en œuvre, parmi lesquels les learning analytics, qui concernent plus spécifiquement les usages pédagogiques.

La mise en place du monitoring consistera à collecter des données et des métriques, afin de pouvoir produire des rapports et générer des tableaux de bord.
Le premier travail à réaliser sur ce sujet portera sur la gestion des logs, pour pouvoir l'intégrer au plus tôt dans les développements et les services déployés.


### Back office
La partie back office n'a pas encore été initiée, mais des orientations ont déjà été établies :

- Séparation complète de l'interface d'administration par rapport au frontend utilisateur, pour des raisons évidentes de sécurité.
L'objectif est de limiter au maximum la surface d'exposition, en renforçant les mécanismes de contrôle d'accès, par exemple via un accès restreint au réseau interne ou la mise en place d'une authentification multifacteur.

- Centralisation du paramétrage de la plateforme, afin de garantir une gestion unifiée et cohérente des paramètres applicatifs. Cette centralisation offrira également des possibilités intéressantes, comme la modification dynamique du paramétrage ou la désactivation ciblée de fonctionnalités, sans interruption de service.

## Interopérabilité et intégration dans l'écosystème
Ce module vise à assurer la bonne intégration de la plateforme dans l'écosystème des établissements. Deux grands axes sont concernés :

 - La constitution de la base d'utilisateurs, en lien avec le module d'authentification et de contrôle d'accès.
 - La dimension pédagogique, au travers :
 - de l'offre de formation (Apogée, Pégase, Esup-ORA) ;
 - de la gestion des stages (Esup-Stage) ;
 - de l'interconnexion avec les plateformes d'apprentissage (LMS) telles que Moodle.

Des échanges, réunions de travail et ateliers ont été menés avec les différents acteurs, en particulier avec l'équipe de PCScol en charge de Pégase.
Une collaboration étroite est également engagée avec l'Université de Caen, qui développe Esup-ORA, pour la formalisation des formations en approche par compétences (APC). Ce projet est désormais intégré au programme Avenirs sur son volet ESR.

Une autre fonctionnalité clé de ce module concerne l'import/export des portfolios. L'objectif est de permettre :
 - la migration de portfolios existants, notamment issus de l'expérimentation Karuta ;
 - la portabilité individuelle : permettre aux utilisateurs de conserver et idéalement transférer leur portfolio vers une autre plateforme si besoin.

Pour certaines de ces problématiques, des standards bien établis existent, comme LTI (Learning Tools Interoperability) pour l'intégration avec Moodle.
Dans d'autres cas, l'avancement dépend des solutions tierces, dont les priorités peuvent ne pas toujours être alignées avec les nôtres.
Enfin certaines pistes sont également envisagées pour la suite, comme l'intégration du protocole SCIM (System for Cross-domain Identity Management) pour la gestion des identités.

{% include img.html
        src="assets/images/pegase_ora_cofolio_objets_metier.png"
        alt="Collaboration avec d'autres projets nationaux"
        width="50%"
        caption="Exemple de collaboration avec des projets nationaux - Travail sur les objets métier"
%}


[TODO] Détailler les autres aspects de l'interopérabilité, avec les services du ministère, etc.

## Conclusion

Le développement de la plateforme de ePortfolio avance de manière structurée, malgré un retard initial sur la partie frontend. Ce temps a été mis à profit pour engager des travaux structurants, tant sur le plan architectural qu'organisationnel. La trajectoire reste bonne : les spécifications frontend vont être finalisées progressivement, et les premiers livrables fonctionnels pourront commencer à être livrés.

Un diagramme de synthèse présente l’état d’avancement actuel par grands modules fonctionnels :

{% include img.html
        src="assets/images/avancement_modules_03_2024.svg"
        alt="Estimation de l'avancement par grand module"
        width="50%"
        caption="Estimation de l'avancement par grand module fonctionnel – mise à jour mars 2024."    
%}
<br/>

Par ailleurs, une feuille de route à moyen terme est proposée, sous forme de macro-jalons, pour donner de la visibilité sur les prochaines étapes clés :

[TODO] Diagramme de type Gantt à maille large – *ex : S1 2025 → Finalisation des spécifications frontend / S2 → Développement / S2–S3 → Intégration / etc.*

L’enjeu des prochains mois sera d’accélérer progressivement le développement tout en maintenant les exigences de qualité, de sécurité et de cohérence, afin de faciliter l’adoption de l’approche par compétences et d’offrir une expérience utilisateur optimale.
