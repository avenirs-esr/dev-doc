---
layout: page
title: Dossier d'Architecture logicielle - module Security - RBAC
permalink: /arch-soft-specif-security-rbac/
up: ../../arch-soft-specif-index/
page_content_classes: table-container
---
[Retour](arch-soft-specif-security.markdown)<br/>
<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Role Based Access Control - RBAC<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 15/09/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>



## Rôle Based Access Control (RBAC)
Le contrôle d'accès est un élément central pour la gestion de la sécurité et le choix qui est fait est de mettre en place un système basé sur un modèle simple, robuste et éprouvé, RBAC : Rôle-Based Access Control. 

Il doit permettre de  :
- Déterminer si un utilisateur peut réaliser une action.
- Lister les rôles/permissions d'un utilisateur.
- Lister les utilisateurs disposant d'un rôle donné, éventuellement pour un scope et un contexte d'application. 
- Lister les utilisateurs disposant d'une permission donnée. 
- [TODO] La notion d'historique est-elle importante ? Est-ce que l'on veut être capable de réaliser les 3 listing pour une date donnée ?
=> Risque de complexifier le système : peut-être dans une première approche passer par la notion de revue de droits.

## RBAC - Fonctionnement

Une action est autorisée ou non en fonction des rôles actifs des utilisateurs, de la ressource impliquée et du contexte d'exécution.<br>
Un rôle peut être assigné à un utilisateur avec un scope qui correspond aux ressources pour lesquelles il s'applique. <br>
Il peut être associé également à un contexte d'application, par exemple une période de validité ou un établissement.  

{% include img.html
        src="assets/images/architecture-security-rbac.png"
        alt="Security - RBAC"
        caption="Rôle Based Access Control"
%}

<br>
### RBAC - Exemple pour le partage d'une ressource
Dans l'exemple suivant un utilisateur veut partager en lecture une de ses ressources, pour une période donnée.<br>
**Remarques:**
- Les libellés associés au rôles et permissions ne sont que des exemples.
- L'exemple ne tient pas compte d'une éventuelle hiérarchie de rôles et permissions.

{% include img.html
        src="assets/images/architecture-security-rbac-ex.png"
        alt="Security - RBAC"
        caption="RBAC - Partage  d'une ressource en lecture"
%}

## Extensions possibles du modèle RBAC
- Utilisation de contraintes, sous forme de prédicat, pour l'assignation des permissions aux rôles et des utilisateurs au rôles.
- Rôle prérequis : prédicat basé sur l'appartenance à un rôle. Exemple appartenance au rôle administrateur pédagogique possible uniquement si appartient au rôle enseignant.
- Rôles qui s'excluent mutuellement, par exemple Pair / Enseignant.
- Nombre max d'assignements à un rôle ; peut-être intéressant pour les super admins.
- Utiliser un modèle réactif, non couplé à la session utilisateur. Cela permettrait par exemple de maintenir en temps réel la liste des partages associés.

## Proposition : classification des actions par niveau de dangerosité

Il pourrait être intéressant d'attributer un niveau de dangerosité aux actions. Cela permettrait de définir des zones dans l'application en fonction de la dangerosité des actions qui y sont menées et d'ajuster les contrôles, logs, alertes, audits, etc. à réaliser. 
[Classification des actions par niveau de dangerosité](arch-soft-specif-security-rbac-levels.markdown)




<br/>[Retour](arch-soft-specif-security.markdown)