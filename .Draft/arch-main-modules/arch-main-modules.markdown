---
layout: page
title: Architecture - Main modules
permalink: /arch-main-modules/
---

# Décomposition du travail

## Liste des Briques nécessaires pour l'e-portfolio

* Brique établissement permettant aux enseignants et aux administrations de définir et valider les offres de formation en APC\
  => U. Caën en cours
* Module de gestion des différents types de compétences (APC, autres ?) avec les situations (saés, situation de stage, situation d'apprentissage, etc...)\
=> à voir avec la brique de maquettage de l'offre de formation en APC de U. Caën 
  * Gérer potentiellement plusieurs référentiels (Compétences du XXIè siècle, à s'orienter, APC, etc...)
  * Il faut prévoir un système permettant la correspondance APC avec le référentiel de compétences RNCP  et ROME 4.0, mais également et éventuellement avec les compétences du SCO
  * Il faut prévoir l'usage d'un référentiel générique et globale (terme de Compétence Nationale ?) d'où les universités peuvent partir pour dériver avec des compléments, suppressions et ajouts, ou éventuellement en reprenant les compétences d'une autre université. 
    * Pouvoir avoir une vision globale des appropriations faites par les universités pour pouvoir contribuer au modèle générique et global par un admin (au choix)
    * Prévoir que les universités puissent contribuer aux référentiels de compétences avec une modération (au choix)
  * Les référentiels de compétences seront hébergés sur un serveur décentralisé afin d'être réutilisés par différents organismes comme nomenclatures. Un intérêt de dériver une compétence et de tracer la compétence d'origine.
* Module d'administration, d'authentification et de gestion de la sécurité => à gérer avec le API-Manager 
  * Gestionnaire d'identité (gestion des identités différentes entre chaque étab / fusion France Connect)\
  // TODO: demander à PC-Scol comment sont gérées les identités/fusion/France Connect
  * Gestion des rôles (lié au profil) pour les droits en gestion globale/par étab
  * Gestion de permissions pour les droits fins (de base matrice associant par défaut des permissions à un rôle + permissions spécifiques) - permissions attribuées à des rôles + permissions fines à des personnes pour certains cas
  * Attention il faut gérer plusieurs structures avec des droits globaux à la plateforme (super-admin) et des droits par établissements, avec possibilité de délégation
  * Cela induit une gestion sur les établissements voir par composante/filière
* Module de gestion des traces / d'entrepôt de données 
  * À tout moment une trace peut être ajoutée, modifiée (complétée, évoluer), supprimée et versionnée
  * Attention si la trace est liée à une compétence évaluée (uniquement dans ce cas ?) il faut une gestion des versions (obligatoire quand évaluée afin de garder un historique du travail évalué, mais peut être à l'initiative de l'utilisateur)\
    => un groupe de travail composé de DPO et de juristes doivent statuer sur le cylce de vie des données
  * Les traces peuvent être diverses et variées, des pièces jointes (médias ou documents) comme des URL, des appels à des services intégrés/intégrables (OpenBadge, test moodle, preuves diverse et variées externes à la plateforme)
  * Prévoir des métadonnées autour des traces car cela peut faire l'objet d'une indexation et de mise à disposition via des plateformes externes (lors d'un transfert des données vers un autre portfolio)
  * Gestion du cycle de vie des traces et leurs versions (nettoyage)
  * Intégration avec le module d'interopérabilité avec les outils pédago (ex: intégration d'une trace d'un LMS, permettre de lancer une activité à partir du portoflio)
* Module de collecte des usages / Monitoring de la plateforme => outils du Data Lakehouse 
  * Statistiques des usages
  * xAPI avec les learning **analytics**
  * Monitoring
  * Gestion des log avec monitoring
  * kafka R ?
* Module/librairie cœur d'interopérabilité avec les outils pédagogiques / vie étab 
  * LMS via LTI
  * ESUP-Stage
    * 1er besoin: récupération des liens Tuteur / Étudiant (si possible et intérêt ?)
    * 2ème besoin: ESUP-Stage a besoin de récupérer les compétences travaillées en APC pour les indiquer dans la convention générée. 
  * POD
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
