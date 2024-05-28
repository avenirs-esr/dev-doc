---
layout: page
title: Dossier d'Architecture logicielle - module Security
permalink: /arch-soft-specif-security/
up: ../arch-soft-specif/
page_content_classes: table-container

---
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Architecture logicielle du module Security.<br/>

**Révision :** 1.0.0<br/>
**Date :** 25/04/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale - Draft<br/>

-----
# Contexte
La prise en compte des problématiques de sécurité est un élément centrale du projet et s'appuie sur la mise en place d'une démarche d'amélioration continue de type [security by design](../security-by-design/index.markdown).

Le portfolio est amené à devenir un élément central dans la vie des enseignants et des étudiants pour les formations en APC. La compromission d'un compte pourrait donc avoir de très lourdes conséquences que ce soit pour l'activité pofessionnelle des enseignants ou la réussite des étudiants et renforcent encore, si besoin était la criticité des problématiques de sécurité.  

# Objectifs
L'objectif est d'assurer le meilleur niveau possible de sécurité et de confidentialité. 
en préventif, un ensemble de procédures et d'outils visent à limiter au maximum les risques, par exemple en intégrant les bonnes pratiques ou en intégrant des audits dans le pipeline d'intégration continue.
Pour le curatif, Il s'agit d'être en mesure de détecter, déterminer l'impact et de corriger le plus rapidement possible les failles de sécurité, les intrusions ou les compromissions.

# Contraintes
Certaines caractéristiques de ce projet rendent les problématiques de sécurité particulièrement complexes :
- Un grand nombre d'utilisateurs potentiels avec des profils variés.
- des fonctionnalités de partage entre utilisateurs sont au coeur du fonctionnement du portfolio. La granularité des partages est assez fine et peut ne concerner qu'un élément spécifique d'un portfolio.
- Un déploiement de type SASS / Hybride. 
- Une forte interconnexion avec des services extérieurs.
- ...

L'envergure du projet impose de mettre en place un système qui permette de suivre et de tracer au plus prêt le fonctionnement de la plateforme. Il faut, par exemple, être en mesure, à un instant t, de lister les permissions accordées à un utlisateur ou les utilisateurs diposant d'une permission donnée. 


## Principaux composants

{% include img.html
        src="assets/images/architecture-security.png"
        alt="Security - Main components"
        caption="Security module - Main components"
%}

## Rôle Based Access Control (RBAC)
Le contrôle d'accès est un élément central pour la gestion de la sécurité et le choix qui est fait est de mettre en place un système basé sur un modèle simple, robuste et éprouvé, RBAC : Rôle-Based Access Control. 

Il doit pemettre de  :
- déterminer si un utilistaeur peut réaliser une action.
- lister les rôles/permissions d'un utlisateur.
- Lister les utilisateurs diposant d'un rôle donné, eventuellement pour un scope et un contexte d'application. 

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
- Rôle prerequis : prédicat basé sur l'appartenance à un rôle. Exemple appartenance au rôle administrateur pédagogique possible uniquement si appartient au rôle enseignant.
- Rôles qui s'excluent mutuellement, par exemple Pair / Enseignant.
- Nombre max d'assignements à un rôle ; peut-être intéressant pour les super admins.
- Utiliser un modèle réactif, non couplé à la session utilisateur. Cela permettrait par exemple de maintenir en temps réel la liste des partages associés.

## Proposition : classification des actions par niveau de dangerosité

Il pourrait être intéressant d'attributer un niveau de dangerosité aux actions. Cela permettrait de définir des zones dans l'application en fonction de la dangerosité des actions qui y sont menées et d'ajuster les contrôles, logs, alertes, audits, etc. à réaliser. 

**Exemple :** 
- Faible : Pas de réel enjeu, concerne ce qui est publique. Par exemple, afficher la liste des portfolios sans en montrer le contenu. 
- Moyenne : Enjeu lié à la confidentialité. Par exemple, lire le contenu partagé d'un portfolio.
- Forte : Concerne les opérations de modification réversibles. La réversibilité implique un versionning ou à minima la possibilité de restaurer à partir de backup. Exemple : réaliser / modifier un retour, modifier le contenu. 
- Critique : opérations dangereuses, difficilement réversibles, à fort effet de bord. Ce niveau implique le renforcement maximal des mesures de sécurité. Exemple : supprimer un portfolio, modifier les droits d'accès, partager en écriture. 

**Remarque :** voir comment peut être intégré la donnée et son niveau de confidentialité.


## Actions utilisateur par type de population

<table>
    <thead>
        <th>Population</th>
        <th>Activité dans l'application</th> 
        <th>Criticité</th> 
        <th>Action RBAC</th>
        <th>Description / remarques</th> 
    </thead>
    <tbody>
     <tr>
            <td rowspan=6>
                Personnel technique 
                <font style="font-size:smaller;color:grey;">Exploitants, super administrateurs</font>
            </td>

            <td>
               Ouvrir le portfolio pour un ou plusieurs établissements  (ou pour une formation, correspondrait à "gestion basique de l'ouverture du portfolio pour une formation")
            </td>
            <td rowspan=6>
            Critique
            </td>
            <td>
                ADM_OPEN_PORTFOLIO_ETAB
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
        
        <tr>
            <td rowspan=1>
                Enseignant
            </td>
            <td>
            1
            </td>
            <td>
            2
            </td>
            <td>
            3
            </td>
            <td>
            4
            </td>
        </tr>

        <tr>
            <td rowspan=1>
                Personnel administratif
            </td>
            <td>
            1
            </td>
            <td>
            2
            </td>
            <td>
            3
            </td>
            <td>
            4
            </td>
        </tr>

    
        <tr>
            <td rowspan=2>
                Etudiant
            </td>
            <td>
            1
            </td>
            <td>
            2
            </td>
            <td>
            3
            </td>
            <td>
            4
            </td>
        </tr>
         <tr>
           
            <td>
            1
            </td>
            <td>
            2
            </td>
            <td>
            3
            </td>
            <td>
            4
            </td>
        </tr>



         <tr>
            <td rowspan=1>
                Externe
            </td>
            <td>
            1
            </td>
            <td>
            2
            </td>
            <td>
            3
            </td>
            <td>
            4
            </td>
        </tr>

    </tbody>
</table>
