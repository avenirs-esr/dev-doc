---
layout: page
title: Dossier d'Architecture logicielle - module Security
permalink: /arch-soft-specif-security/
up: ../arch-soft-specif-index/
page_content_classes: table-container
---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Security.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Contexte
La prise en compte des problématiques de sécurité est un élément central du projet et s'appuie sur la mise en place d'une démarche d'amélioration continue de type [security by design](../security-by-design/index.markdown).

Le portfolio est amené à devenir un élément central dans la vie des enseignants et des étudiants pour les formations en APC. La compromission d'un compte pourrait donc avoir de très lourdes conséquences que ce soit pour l'activité possessionnelle des enseignants ou la réussite des étudiants et renforcent encore, si besoin était la criticité des problématiques de sécurité.  

## Objectifs
L'objectif est d'assurer le meilleur niveau possible de sécurité et de confidentialité. 
en préventif, un ensemble de procédures et d'outils visent à limiter au maximum les risques, par exemple en intégrant les bonnes pratiques ou en intégrant des audits dans le pipeline d'intégration continue.
Pour le curatif, Il s'agit d'être en mesure de détecter, déterminer l'impact et de corriger le plus rapidement possible les failles de sécurité, les intrusions ou les compromissions.

## Contraintes
Certaines caractéristiques de ce projet rendent les problématiques de sécurité particulièrement complexes :
- Un grand nombre d'utilisateurs potentiels avec des profils variés.
- des fonctionnalités de partage entre utilisateurs sont au coeur du fonctionnement du portfolio. La granularité des partages est assez fine et peut ne concerner qu'un élément spécifique d'un portfolio.
- Un déploiement de type SASS / Hybride. 
- Une forte interconnexion avec des services extérieurs.
- ...

L'envergure du projet impose de mettre en place un système qui permette de suivre et de tracer au plus prêt le fonctionnement de la plateforme. Il faut, par exemple, être en mesure, à un instant t, de lister les permissions accordées à un utilisateur ou les utilisateurs disposant d'une permission donnée. 


## Principaux composants

{% include img.html
        src="assets/images/architecture-security.png"
        alt="Security - Main components"
        caption="Security module - Main components"
%}

## Base de données

{% include img.html
        src="assets/images/architecture-security-db.png"
        alt="Security - Database"
        caption="Security module - Database"
%}

## Rôle Based Access Control (RBAC)
Le contrôle d'accès est un élément central pour la gestion de la sécurité et le choix qui est fait est de mettre en place un système basé sur un modèle simple, robuste et éprouvé, RBAC : Rôle-Based Access Control. 

Il doit permettre de  :
- Déterminer si un utilisateur peut réaliser une action.
- Lister les rôles/permissions d'un utilisateur.
- Lister les utilisateurs disposant d'un rôle donné, éventuellement pour un scope et un contexte d'application. 
- Lister les utilisateurs disposant d'une permission donnée. 
- [TODO] La notion d'historique est-elle importante ? Est-ce que l'on veut être capable de réaliser les 3 listing pour une date donnée ?

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

**Exemple :** 
- Faible : Pas de réel enjeu, concerne ce qui est publique. Par exemple, afficher la liste des portfolios sans en montrer le contenu.  
- Moyenne : Enjeu lié à la confidentialité. Par exemple, lire le contenu partagé d'un portfolio. 
- Forte : Concerne les opérations de modification réversibles. La réversibilité implique un versionning ou à minima la possibilité de restaurer à partir de backups ou de réinitialiser. Exemple : réaliser / modifier un retour, modifier le contenu.
- Critique : opérations dangereuses, difficilement réversibles, à fort effet de bord. Ce niveau implique le renforcement maximal des mesures de sécurité. Exemple : supprimer un portfolio, modifier les droits d'accès, partager en écriture.

**Remarque :** voir comment peut être intégré la donnée et son niveau de confidentialité.

### Actions utilisateur par type de population et permissions associées

**Remarques :** 
- **Travail en cours**, premiers éléments pour base de discussion. Les listes seront à compléter une fois le principe validé.
- Un travail sur l'organisation hiérarchique des rôles et permissions devra être effectué lorsque la liste sera plus avancée.

<table>
    <thead>
        <th>Population</th>
        <th>Activité dans l'application</th> 
        <th>Criticité</th> 
        <th>Action RBAC</th>
        <th>Permission</th>
        <th>Description / remarques</th> 
    </thead>
    <tbody>
     <tr>
            <td rowspan=2>
                Personnel technique 
                <font style="font-size:smaller;color:grey;">Exploitants, super&nbsp;administrateurs</font>
            </td>

            <td>
               Ouvrir le portfolio pour un ou plusieurs établissements  (ou pour une formation, correspondrait à "gestion basique de l'ouverture du portfolio pour une formation")
            </td>
            <td rowspan=2>
            Critique
            </td>
            <td>
                ADM_OPEN_PORTFOLIO_ETAB
            </td>
            <td rowspan=2>
               ADMIN 
            </td>
            <td>
               Opération préalable à la mise en oeuvre du portfolio pour un établissement, par exemple pour que l'administrateur pédagogique puisse ouvrir le portfolio pour une formation. 
            </td>
            
        </tr>
        <tr>
            <td>
                Assigner/Révoquer des rôles aux utilisateurs
            </td>
                    
            <td>
                ADM_ASSIGN_ROLE / ADM_DIMISS_ROLE

            </td>
            <td>
                Par exemple assigner/révoquer le rôle d'administrateur pédagogique à un enseignant (ou un personnel administratif ?)
            </td>
            
        </tr>

         <!-- <tr>
            <td>
            </td>
        
            <td>
                
            </td>
            <td>
                
            </td>
            
        </tr> -->

        <tr><td colspan=6 style="background-color: lightgrey;"></td></tr>
        
       
        
        <tr>
            <td rowspan=6>
                Enseignant<br/>
                <font style="font-size:smaller;color:grey;">Titulaires et vacataires ?</font>
            </td>
            
            <td>
                Ouvrir le portfolio pour une formation
            
            </td>

            <td> 
                Critique
            </td>

            <td>
                OPEN_PF_FORM
            </td>
            <td>
                ADMIN_P 
                <br/><font style="font-size:smaller;color:grey;">(Administrateur pédagogique)</font>
            </td>
            <td>
                Ouverture du portfolio pour les étudiants, probablement par le responsable de la formation.<br/>
                L'administrateur pédagogique doit pouvoir effectuer un certain nombre de paramétrages en amont et visualiser la vue étudiante pour vérification.

            </td>
        </tr>

        <tr>
            <td>
                Consulter la liste de portfolios mis en accès
            
            </td>

            <td> 
                Faible (moyenne&nbsp;?)
            </td>

            <td>
                CONSULT_SHARED_PF_LIST
            </td>
            <td>
                SEE
            </td>
            <td>
                Affichage d'une liste uniquement. La criticité dépend des données affichées au niveau de la liste.
            </td>
        </tr>
        <tr>
            <td>
                Consulter un portfolio mis en accès
            </td>

            <td>
                Moyenne
            </td>

            <td>
                CONSULT_SHARED_PF
            </td>

            <td>
                READ
            </td>

            <td>
            </td>
        </tr>
        <tr>
            <td>
                Effectuer un retour sur un portfolio
            </td>

            <td>
                Forte
            </td>

            <td>
                FEEDBACK_SHARED_PF
            </td>

            <td>
                WRITE
            </td>
            
            <td>
            </td>
        </tr>

        <tr>
            <td>
                Définir une activité
            </td>

            <td>
                Forte
            </td>

            <td>
                DEFINE_ACT
            </td>

            <td>
                WRITE
            </td>
            
            <td>
                L'activité n'est visible que de l'enseignant.
            </td>
        </tr>
        
        <tr>
            <td>
                Publier une activité
            </td>

            <td>
                Critique
            </td>

            <td>
                PUBLISH_ACT
            </td>
            <td>
                WRITE
            </td>

            <td>
                La criticité est maximale car l'activité devient visible pour les étudiants qui peuvent commencer à la travailler. L'effet de bord est important puisqu'il impacte plusieurs utilisateurs et est difficilement réversible s'ils ont déjà commencé à travailler dessus.
            </td>
        </tr>

        <tr><td colspan=6 style="background-color: lightgrey;"></td></tr>

        <tr>
            <td rowspan=1>
                Personnel administratif
            </td>
            
            <td>
                A voir si ça a du sens.<br/>
                Est-ce qu'un chargé de formation, peut par exemple, déléguer certaines tâches à ses secrétaires ?<br>
                (Risque de diffusion de login / mdp dans le cas contraire)
            </td>
            
            <td>
            </td>
            
            <td>
            </td>
            
            <td>
            </td>
        </tr>
        
        <tr><td colspan=6 style="background-color: lightgrey;"></td></tr>

    
        <tr>
            <td rowspan=4>
                Etudiant
            </td>
            
            <td>
                Partager en écriture tout ou partie du portfolio
            </td>
            
            <td>
                Critique
            </td>
            
            <td>
                SHARE_WRITE_PF
            </td>
            
            <td>
                SHARE
            </td>

            <td>
                Pour des considérations de sécurité, le choix qui est fait est de différencier les types de partage au niveau des action, plutôt que d'avoir une seule action en l'utilisant via des rôles associés à des scopes différents.
            </td>
        </tr>
        <tr>
            
            
            <td>
                Partager en lecture tout ou partie du portfolio
            </td>
            
            <td>
                Moyenne
            </td>
            
            <td>
                SHARE_READ_PF
            </td>
            
            <td>
                SHARE
            </td>

            <td>
            </td>
        </tr>
        <tr>
           
            <td>
                Effectuer un retour
            </td>
            
            <td>
                Critique
            </td>
            
            <td>
                DO_FEEDBACK
            </td>
            
            <td>
                WRITE
            </td>

            <td>
            </td>
        </tr>
        <tr>
           
            <td>
            </td>
            
            <td>
            </td>
            
            <td>
            </td>
            
            <td>
            </td>
        </tr>
        
        <tr><td colspan=6 style="background-color: lightgrey;"></td></tr>

         <tr>
            <td rowspan=1>
                Externe
            </td>

            <td>
            </td>

            <td>
            </td>

            <td>
            </td>

            <td>
            </td>
        </tr>

    </tbody>
</table>
<br/><br/> <!-- [TODO] revoir le css pour les tableaux (.table-container)-->

### Roles et permissions associées

Une fois le tableau précédent complété, il est possible de définir les rôles en leur associant les permissions qui y son rattachées. La criticité d'un rôle correspond au niveau le plus haut des permissions qui lui sont rattachées.

<table>
    <thead>
        <th>Rôle</th>
        <th>Permissions</th> 
        <th>Criticité</th> 
        <th>Exemple de scope</th>
        <th>Description / remarques</th> 
    </thead>
    <tbody>
        <tr>
            <td>
                OWNER
            </td>
            <td>
                <ul>
                    <li>SEE</li>
                    <li>READ</li>
                    <li>WRITE</li>
                    <li>SHARE</li>
                    <li>DELETE</li>
                </ul>
            </td>
            <td>
                Critique
            </td>
                
            <td>
                Resource: idDuPortfolio<br/>
            </td>
            <td>
            </td>
        </tr>

        <tr>
            <td>
                PAIR
            </td>
            <td>
                <ul>
                    <li>SEE</li>
                    <li>READ</li>
                    <li>WRITE</li>
                   
                </ul>
            </td>
            <td>
                Critique
            </td>
                
            <td>
                Resource: idElementPartagé<br/>
            </td>
            <td>
            </td>
        </tr>

    </tbody>
</table>


## Premières expérimentations 

# Modèle de données


{% include img.html
        src="assets/images/avenirs_access_control_db.png"
        alt="Security - Main components"
        caption="Security module - Main components"
%}

# Données de test


{% include img.html
        src="assets/images/access_control_fixtures.png"
        alt="Security - Main components"
        caption="Security module - Main components"
%}