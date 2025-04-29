---
layout: page
title: Architecture - Main modules
permalink: /arch-main-modules/
---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Décomposition du travail.<br/>
**Date de révision :** 12/06/2024<br/>
**Auteur :** J. Gribonvald<br/>
<br/>

# Décomposition du travail

Cela consiste à définir la liste des outils/briques applicatives nécessaires consituant l'écosystem de l'e-portfolio.

## Aide à la formalisation d'une offre de formation en APC - ESUP-ORA

Il s'agit d'un outil pour les établissements permettant aux enseignants et aux administrations respectivement de définir et valider les offres de formation en APC.

Développement en cours par U. Caën.

## Base des référentiels

Outil / base centralisée récupérant les référentiels des offres de formation en APC de tous les établissements.
En lien avec ESUP-ORA et PC-SCOL pour la récupération des référentiels et leurs identifiants.

Le but de cet outil est de:

* conserver l'historique des offres de formations de tous les établissements
  * Nécessaire pour que les étudiants puissent pointer sur les référentiels de leurs formations
  * Servira pour les VAE
  * Permet de fournir des exemples à d'autres établissements afin de constituer leur propre référentiel
* consolider, permettre et faciliter l'alimentation de l'e-portfolio
  * Il est nécessaire de consolider les données récupérées d'ESUP-ORA et Pégase, car Pégase fourni les identifiants utiles pour associer les évaluations à remonter.
* offrir un accès au catalogue des formations aux différents partenaires:
  * ONISEP
  * CPF
  * EUROPASS
  * HCERES
  * Passeport Compétences ?
* potentiellement, récupérer des autres référentiels de compétences pour base de connaissance à l'e-portfolio
  * compétences du XXIè siècle
  * compétences à s'orienter
  * autres à préciser
* Sauvegarde, gestion d'un historique et accès à un "store" des SAÉs

Un référentiel APC aura toujours des correspondances d'équivalence avec un référentiel RNCP ainsi que ROME4.0.

## Outil d'imports/export

Système de gestion des different types d'imports et export possibles de façon contextualisée.
En effet chaque établissement ayant ses propores outils/services il est nécessaire de pouvoir adapter selon le cas les scripts et outils mis à disposition afin d'importer ou expoter des données.

Voici un panel d'exemples identifiés:

* Alimentation des formations, utilisateurs à partir de Pégase
* Export des notes vers Pégase
* Alimentation des formations, utilisateurs à partir d'Apogée
* Export des notes vers Apogée
* 

## Paramétrage de la plateforme / Back Office

Un modèle de données avec des APIs spécifiques à la gestion sont à prévoir.

### Catalogue de services

Module de rescencement et gestion des services inter-connectés à la plateforme, spécifiques à des partenaires et surtout aux établissements/composantes/site/formations.

Le but étant d'établir une liste de services avec l'usage contextualisés, les méthodes et paramètres de configuration afin d'être interropérable/connecté/articulé avec le service.

Liens avec:
* LMS: Moodle direct et LTI
  * Lancer une activité moodle et en récupérer le résultat des productions
* ESUP-Pod
  * Dépôt et intégration d'un lien vidéo
* Nextcloud
  * Utilisation d'un document déposé sur le nextcloud
  * Travail sur des documents via une suite d'édition collaborative
* ESUP-Stage
    * le portoflio peut récupérer des liens Tuteur / Étudiant (si possible et intérêt ? par certain que celà soit possible)
    * ESUP-Stage a besoin de récupérer les compétences travaillées en APC pour les indiquer dans la convention générée.
* FranceBadge
  * Recherche, création et récupéation de badges
* Stores de SAÉs (interne ou en lien avec des offres éditeurs)
* Réseau-pro ou service équivalent d'éditeur (dans le cadre de SAP proposer des stages, alternances, etc...)
* Intégration d'IA (usage d'un service d'IA, potentiellement solutions éditeur ou souverraine telle Aristote IA)
* etc...

### Administration Globale de la plateforme

Cela revient à la gestion des paramétrages globaux + par établissements/composantes/sites/formations intégrés. Droit fin à prévoir.

* Gestion basique de l'ouverture du portfolio pour une formation
* Listing des responsables autorisés à l'ouverture des e-portfolios par formations
* Listing des outils tiers URL/EndPoints d'API (LMS) afin d'interfacer l'e-portfolio
* Définission des différents types d'import export

Configuration des paramètres de sécurité:

* Gestion des clés/jetons de sécurisation des flux de données
* Système d'authentification
* Gestion des identités
* Définission des rôles et permissions (matrice de droits)

Puis autres informations de paramétrages diverses et variées.

l'API-Manager se base sur ces éléments et est central en terme de gestion de la sécurité.

La liste sera compléter au fil du temps.

### Administration Locale de la plateforme

Cette partie permet la délégation de droits fins afin de laisser la main aux administrateurs locaux de l'établissement.
En lien avec la matrice droits, il sera nécessaire d'indiquer ce qui sera délégué.

## Pilotage de la plateforme

La mise en place d'outils pour le pilotage d'une plateforme est d'un enjeux majeur puisque cela permet des prises de décision quand aux orientations d'évolutions.
Mais également cela à pour rôle de permettre le monitoring, d'accroitre la sécurité et d'identifier rapidement les éventuels problèmes facilitant ainsi les interventions.

Composé par:
* Collecte de données liées aux comportements utilisateurs
* Collecte de **Learning Analytics** via xAPI
* Collecte d'informations sur les services (health-check, metrics, logging, auditing, via actuator/JMX) et monitoring
* Tableaux de bords
* Tableaux de statistiques
* Système d'alertes / notification

Cela passe par des outils de type greylog, grafana, stack ELK, etc... On peut parler d'outils de DataLakeHouse.

## Sécurisation

Défini les éléments composant la sécurité de la plateforme. Basé sur les principes de service d'IAM (Identity Access Manager).

L'API-Manager s'appuie sur ces services au cœur de l'écosystème.

### Identité

Système de gestion des comptes/profils/identifiants.

Il est à prévoir un système de gestion mais également de rapprochement des identités avec différentes cinématiques:
* Gestion d'identifiants opaques ou internes spécifiques à tous les outils/plateformes tiers (Université A, Pégase/Apogée, EduConnect, FranceConnect, Moodle, etc...).
* Identités changeant entre les différents établissements (université A et B)
* Suivi et gestion des différentes identités obtenues tout au long de la vie (Éduconnect, Université A, Université B, FranceConnect)

À tout moment l'utilisateur doit pouvoir gérer les informations de son profil en accord avec les services et faire le rapprochement de plusieurs comptes / id de différentes plateformes.

Exemples de cinématiques:
* L'étudiant arrivant dans une université souhaitant importer dans son nouveau portfolio ses données du portfolio du SCO. À partir du portfolio du supérieur, en étant authentifié, il devra également s'authentifier sur le portail du SCO afin de pouvoir faire le lien avec le bon compte. Il devra ensuite indiquer les informations à récupérer.
* La cinématique précédente est également valable entre le mmonde du travail et le supérieur, un lien entre le compte FranceConnect et l'identité dans son ancien compte universitaire devra être réalisé.
* Un rapprochement d'identité sera également à réaliser entre les outils suivants des universités:
  * Moodle
  * Nextcloud
  * ESUP-POD
  * OpenBadge


### Authentification

Un service gérant et validant l'authentification des utilisateurs. Le service utilisé permettra aussi de pouvoir être fournisseur d'identité au près de partenaires ou de services spécifique si cela est nécessaire.

L'authentification sera majoritairement déléguée à un service de SSO de la fédération Renater. Une possibilité d'authentification locale à la plateforme pour des besoins d'administration est à prévoir ainsi que des systèmes permettant une authentification faible à partir d'un lien unique, un qr-code pour des publics "secondaires" comme les maîtres d'apprentissage et tuteurs de stage.


### Autorisation

Un service de gestion des autorisations. Service basé sur une gestion de profils/rôles et persmissions. Il peut être à prévoir plusieurs ensembles de permissions propres à différents domaines d'usages (ou modules) voir différentes granularités.

On peut représenter l'ensemble des droits et persmissions sous forme de matrice.

## Entrepôt de données

Ce service est nécessaire et utile à la gestion des traces / preuves déposées par les utilisateurs. 
Mais également d'un point de vue d'administration afin de facilité la gestion du cycle de vie des différents type de contenus déposés / réalisés.
Des règles métiers spécifiques sont à implémenter en tenant comptes des différents aspects légaux commes fonctionnels de chaque universités, même si des règles nationales existent, il peut y avoir un régleage à apporter en fonction de l'établissement/composante/site/formation.

Quelques exemples de contraintes/fonctionnalités:
* Toutes les données déposées doivent être liées à une gestion de versions
  * Obligatoire dès lors que la trace devient une preuve, selon les feedback, les niveaux de compétences travaillés, la trace peut être travaillée continuellement. Donc afin de garder un historique du travail évalué, "poussé" un lien de version doit être posé. 
  * La définition d'une version peut aussi être à l'initiative de l'utilisateur \

=> Un groupe de travail composé de DPO et de juristes doit définir/statuer le cylce de vie des données
* À tout moment une trace peut être ajoutée, modifiée (complétée, évoluer), supprimée et versionnée
* Les traces peuvent être diverses et variées, des pièces jointes (médias ou documents) comme des URL, des appels à des services intégrés/intégrables (OpenBadge, test moodle, preuves diverse et variées externes à la plateforme)
* Prévoir des métadonnées autour des traces car cela peut faire l'objet d'une indexation pour permettra la recherche et de mise à disposition via des plateformes externes (lors d'un transfert des données vers un autre portfolio)
* Gestion du cycle de vie des traces et leurs versions (nettoyage)
* Intégration avec le module d'interopérabilité avec les outils pédago (ex: intégration d'une trace d'un LMS, permettre de lancer une activité à partir du portoflio)

## Messaging

Socle pour la gestion :
* des notification / alertes
* des demandes de feedback /des échanges entre utilisateurs
* mise en relation

## Gestion des aspects légeaux de la plateforme

### CGU

Adaptables à l'établissement/composante/site/formation

### Autres ?



## E-portoflio

L'approche du portfolio vue jusqu'à maintenant était principalmeent centrée sur l'utilisateur, mais dans le cadre de projets en groupes il faut pouvoir fournir des outils permettant de générer/gérer/éditer des traces en mode collaboratif. Soit, une trace n'appartient pas uniquement à une seule personne, 
//TODO question à poser: est-ce un travail dans le cadre d'une SAé ? Il faudrait à minima pouvoir définir un cadre du travail en commun afin de faciliter les paramétrages et les éviter aux utilisateurs

### 1. Fonctionnalités Thématiques

#### .1 Compétences

Réflexivité, suivi et évaluation (issues des référentiels de formation, d'autres personnalisées par l'étudiant, et d'autres référentiels possibles par exemple transversaux)

#### .2 Traces

création et gestion de ses traces et de pages libres (individuelles et collectives)

#### .3 Activités de Mises en Situations (AMS)

activités de mises en situations (= saé, stages, alternance, autres mises en situations…) à laquelle l'étudiant pourra associer ses traces et ses pages, avec évaluations différentiables quand une mise en situation aborde plusieurs compétences.

#### .4 Gestion des expériences

formations académiques, autres formations, professionnelles, personnelles...

#### .5 Projet de vie

connaissance de soi, exploration de futurs possibles, bâtir son projets...
En lien avec la plateforme ONISEP.

#### .6 Synthèses et CVs

permettre à l'étudiant de réaliser des pages de CV ou "portfolio" en mode brouillon, publié, partagé par lien unique et gestion de versions

### 2. Fonctionnalités Transversales

Modules et gestions

#### .1 Notifications

Mise en avant d'évènements / notifications.
Un éditeur de préférences pour les utilsiateurs


#### .2 Agenda

#### .3 Traces

#### .4 Widgets

La liste des widgets à afficher en page d'accueil pour un aperçu est configurable par l'utilisateur.
Voici un panel des widgets:
* agenda des actions en cours/à réaliser prochainement
* 

#### .5 Partages

Il existe plusieurs façons de partager, la plupart sont automatiques par rapport au portfolio de l'étudiant avec les enseignants et tuteurs de stages / maître d'apprentissage en lien avec la formation. Toutefois certaines granularités sont à prévoir, un enseignant d'une SAÉ n'a pas à voir le travail pour une autre SAÉ où il n'intervient pas.
Par contre d'autres façons de partager sont à définir et à gérer par l'étudiant par rapport à ces notions:

* Traces de travaux en groupes
* CV
* Projet de vie avec un personnel d'orientation

Et la façon de partager peut se faire de différentes façons:

* Ciblé et authentifié
* Via lien unique :
  * à durée limitée
  * protégé par mot de passe
  * génération d'un qr-code à présenter
  * avec gestion des droits: feedback, évaluation, autre ?

#### .6 Communication

Comprend la notion de Feedbacks / communication entre utilisateur / mise en relation 

Synthétiser une vue des échanges et canaux de communication en plus des fonctionnalités intégrées au différents endroits de l'outils.



## Brouillon des différentes briques

totalement à réorganiser


  
* Module de messaging (feedback/évaluation/mise en relation/évènements/notifications) => contact avec U. Bordeaux sur le sujet fregate ?
  * gestion d'évènements (kafka)
  * permettre le dialogue entre étu, enseignants et autres personnes (orientation par exemple), est-ce au travers des feedback uniquement ou lors d'une sollicitation dans un cadre précis du portoflio ?
* Module Portoflio utilisateur APC, Projet de vie, CV
  * Fonctionnement APC
  * Travail sur le projet de vie
  * Travail sur le CV, voir l'articulation avec le MCF (Mon Compte de Formation) de la Caisse des Dépôts ou l'Europass qui proposent cette fonctionnalité.
  * Permettre la vue sous forme de cartes mentales du parcours en APC et autres élements (imbrication des traces dans les travaux réflexifs par exemple)
  * Utilité ? Module de gestion de l'identité/profile avec gestion de permissions sur qui voit quoi, avec exposition potentielle en public via le portfolio de présentation / CV
  * Gestion de template UI, UI qui peuvent évoluer et être à adapter en fonction du contexte utilisateur, d'une personnalisation, donc potentiellement plusieurs contenus/formes de pages en fonction de l'utilisateur, avec des widgets....
* Page d'accueil
  * Gestion d'un affichage niveau débutant et avancé (le niveau avancé est quand l'utilisateur ajoute des contenus/briques/widgets)\
    => Niveau avancé avec ajout d'un agenda par exemple, l'utilisateur peut customiser son interface
  * Gestion des notifications
  * Accès SAés, compétences, stages, traces
  * Didacticiel, gamification
* Module d'import/export des portfolios, utile pour gérer:
  * la prise en compte en "import" des données du portfolios du SCO, du monde du travail et de l'europass
  * la récupération des données utilisateurs (RGPD)
* Module d'alimentation et d'export de données\
  => il faut prévoir une alimentation et un export via API et des clients batch pour chacun des outils. Il faudra donc prévoir une gestion de configuration pour indiquer les clients à utiliser:
  * outils de vie scolaire, depuis/vers Apogée ou Pégase ou complémentaires
  * des clients divers et variés custom à l'établissement
  * gestion du transfert de karuta => version indus
  * alimentation et export ESR/SCO/PRO/EUROPASS
  * Récupération des données utilisateur
* Module de gestion des CGU adaptées aux formations et établissements (médecine, en sécurité (pb de ne pas compromettre des infos, etc...)) - Pourquoi pas des CGU générales mais avec une customisation établissement => valider avec un service juridique
* Module de gamification
* Assistance


## Fonctionnement avec le SCO

Tentative de centralisation des datas au format json sur un data-lake, le portfolio utiliserait et pousserait les infos dans ce système. PB qui se pose est que les données appartiennent aux univ, donc seulement certaines parties seront mises en commun, mais les données utilisateurs seront uniquement à l'initiative de l'utilisateur.

## Travail et métier liés aux différentes réalisations

* WebDesigner UI/UX pour définir les écrans et les cinématiques utilisateur
* Admin Sys + data engeneer pour la gestion d'un data LakeHouse
* Modélisation abstraite de l'objet compétence permettant:
  * de définir (d'instancier) différents types de compétences (on peut modéliser sous forme d'arbre infini mais dont le métier fixera les limitations),
  * en lien avec les apprentissages (par abstraction apprentissage = compétence mais considéré comme élément sous-jacent)
  * avec potentiellement des niveaux sur cet objet abstrait
  * lié à de potentielles évaluations (de différents types: note, niveau, type énuméré, etc...).\
   => Nécessite une personne qui deviendra un référent/expert métier pour spécifier le modèle objet, capable auditer d'Éric et Aurélie (autre personne ?) et devra tester la modélisation/l'instanciation de compétences (cela reviendrait à renseigner des référentiels pour les tester).
* Expert métier Pégase (PC-Scol) / Apogée (AMUE) pour l'interopérabilité des PF:
  * Alimentation de la PF de portfolio: Peuplement des utilisateurs, des référentiels de formations/compétences, établissements et composantes
  * Alimentation des outils Apogée et Pégase avec remontée des notes (potentiellement)
* Spécification des différentes statistiques, tableaux de bords et learning analytics. Nécessite un expert métier pour la spécification des event xAPI à produire, ainsi que d'un data scientist/dataOps/data engeneer pour la production et l'exploitation des statistiques d'usage ainsi que de l'amélioration de la plateforme.
* Intégration d'outil de type LMS avec l'usage potentiel des protocoles LTI (expert métier), le but étant de définir les spécifications d'interaction et de récupération des données produites comme preuves (ou de lancer une activité à faire dans Moodle). Travail de l'U. d'Amiens ou autre université sur un module Moodle dans karuta.
* Intégration avec des outils tiers: OpenBadge -> experts métiers spécifiant les protocoles/webflow
* Spécifier l'administration et la gestion des rôles et permissions en fonction du CDC fonctionnel, besoin d'une expertise fonctionnel pour spécifier techniquement ce module.
* Besoin d'un développeur référent et ayant une expertise RGAA
* Spécialiste capable de comprendre le modèle de données xml du portfolio karuta pour la migration vers la PF indus. Ce même spécialiste devra être capable de spécifier le modèle des métadonnées des portfolios de la version Indus.
* Pour le travail avec le SCO, il est prévu une plateforme de stockage de données au format json dans un data-lake - stockage objet de type S3. Il sera utile de dissocier les données à usages ponctuels des données d'usage courant pour spécifier le type de stockage et donc faire des économies d'échelle
