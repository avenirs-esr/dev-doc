 <img src="https://avenirs-esr.github.io/dev-doc/assets/images/avenir-esr-logo_small.jpg"> <img src="https://avenirs-esr.github.io/dev-doc/assets/images/esup-portail-logo_small.png"/> 
 # Avenirs-ESR / ePortfolio - Dossier architecture logicielle : Module Back Office 

---
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Back Office.<br/>

**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>

-----
# Contexte
Ce module porte les fonctionalités d'administration du site, des APIs ePortfolio et de la gestion de l'interropérabilité.

# Objectifs
Les fonctionalités d'administration doivent permettre aux administrateurs de réaliser les opérations de paramétrage du système. 
Pour des questions de sécurité l'interface utilisateur d'administratino est séparée de celle du portfolio. Cela permettra de réaliser un renforcement de la sécurité, notament via un renforcement des restriction d'accès. Cette partie ne concerne que les administrateur du système portfolio et non les administrateurs pedagogiques. 

Ce module contient également la partie API spécifique ePortfolio, accessible aux utilisateurs via le front office.

Les fonctions d'interroperabilités permettront d'exporter et d'importer les portfolios depuis et vers différents systèmes : 
- portfolio professionnel,
- portfolio scolaire,
- Karuta, etc.
L'objectif principal est que le portfolio d'un utilisateur puisse le suivre au cours des différentes étapes de sa vie.
L'import des utilisateurs doit permettre de créer les comptes du portfolio.

# Containtes
La première contrainte concerne la sécurité. Les fonctionnalités associées à l'administration du site sont très sensibles et doivent être sécurisées au maximum et tracées.
Pour la partie interropérabilité est très dépendantes des systèmes et des implémentations en place dans les établissements. Ce qui est envisagé est de fournir des scripts ou demi-clients adaptables pour faciliter le travail des établissements.
Il pourrait être intéressant de développer un format d'export normalisé qui pourrait devenir un standard de fait. Ce point êt évoqué dans le dossier des fonctionnalités attendues du wp1.
Les services peuvent être accessible en mode SASS ou directment dans les établissements. Un catalogue de service doit permettre de déterminer les modalités d'accès à un service donné.

L'importation doit être réalisée à la demande pour un utilisateur ou un groupe d'utilisateurs par un administrateur pédagogique ou le responsable d'une formation. L'objectif est de ne pas charger la plateforme inutilement.


## Principaux composants
<img src="../../assets/images/architecture-back-office.png" alt="Back Office" />
