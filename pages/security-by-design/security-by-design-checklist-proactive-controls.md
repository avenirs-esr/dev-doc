---
layout: page
title: Security/Privacy by design - Mesures proactives
permalink: /security-by-design-checklist-proactive-controls/
page_content_classes: table-container
---
<div style="border: 2px dashed #e67e22; background: #fcf3e6; padding: 1em; margin: 1em 0; font-weight: bold; color: #e67e22;">
  ⚠️ Cette section est en cours de construction.
</div>
---
**Module :** Avenirs-ESR / ePortfolio - Mesures proactives<br/> 
**Objet :** Liste de mesures proactive à intégrer dans tous les développements<br/>

**Révision :** 1.0.0.<br/>
**Date :** 10/04/2024.<br/>
**Auteur :** A. Deman.<br/>
**Commentaire :** Version initiale - Draft

---

## Sources
[OWASP Top Ten Proactive Controls 2024](https://owasp.org/www-project-proactive-controls/v4/en/introduction)



### Points de vérification
- Contrôle d'accès (ou Autorisation): [C1: Implement Access Control](https://owasp.org/www-project-proactive-controls/v4/en/c1-accesscontrol)


    1. Définir le contrôle d'accès dès les début. -> [TODO] Choisir le pattern (RBAC). Probablement RBAC (Role Based Access Control) 
    2. **Tous les accès doivent passer par le contrôle d'accès.**
    3. Utiliser une seule implémentation : éviter l'exploitation d'un implémentation plus faible ou défaillante.
    4. Deny par défaut : toutes les requêtes non spécifiquement autorisées doivent être rejetées.
    5. Respecter le principe de moindre privilège : créer un rôle privilégié pour chaque fonction critique et éviter un compte administrateur avec tous les privilèges.
        - Just-in-time (JIT) et just enough access (JEA) : donner juste l'accès nécessaire et pour un temps donné.
    6. Pas de rôle codé en dur (e.g. user.hasRole("ADMIN")) 

- Chiffrement : [C2: Use Cryptography the proper way](https://owasp.org/www-project-proactive-controls/v4/en/c2-crypto). 
    1. Ne pas faire transiter les données en clair.
    2. Utiliser des algorithmes de chiffrement standard et éprouvés.
    3. Les données sensibles dont stockées chiffrées, notament les données personnelles des utlisateurs.
    4. Stocker les mots de passes chiffrés (cf point 3).
    5. Gérer les clés secrètes
       1. protéger leur accès.
       2. Logguer leur accès.
       3. Utiliser des clés indépendentes s'il y en a plusieurs
       4. Préparer les mise à jours des clés (changement d'algorithme, rotation/révocation des clés)
- Entrées utilsateur et gestion des exceptions : [C3: Validate all Input & Handle Exceptions](https://owasp.org/www-project-proactive-controls/v4/en/c3-validate-all-input)
Risques: SQL injection, Remote Command Injection. Découlent d'une confusion, au niveau de l'application, entre données utilisateur et commande à exécuter.
    1. Vérification syntaxique : longueur, type de données, etc. 
    2. Vérification sémantique : intervalle de valeurs, date de début avant date de fin, etc. 
    3. Repose sur plusieur mécanismes : filtrage, echappemet, requêtes paramétrées, renforcement, etc (défense en profondeur).
    4. Vérification côté client et côté serveur.
    5. [Mass Assignment](OWASP Mass Assignment Cheat Sheet) : éviter le binding automatiques entre les paramètres HTTP et les objets côté serveur ou le limiter [TODO] intégrer les recommandation de cette cheat sheet.
- [C4: Use Secure Architecture Patterns](https://owasp.org/www-project-proactive-controls/v4/en/c4-secure-architecture)
    [TODO] documenter les principaux patterns

- Utiliser des configuations sécurisées par défaut : [C5: Secure By Default Configurations](https://owasp.org/www-project-proactive-controls/v4/en/c5-secure-by-default.html) 
  
- Gestion des dépendances : [C6: Keep your Components Secure](https://owasp.org/www-project-proactive-controls/v4/en/c6-use-secure-dependencies.html)
    1. Utiliser des sources officielles 
    2. Favoriser les dépendances les plus utilisées.
    3. Utiliser des dépendances activement maintenues.
    4. Utiliser des version stables.
    5. Favoriser la simplicité (moins de fonctionnalités, moins de dépendances).
    6. Pour les dépendances open source utiliser une application de test static (SAST : Static Application Security Testing) ou un logiciel d'analyse de composison (SCA: Software Composition Anldalysis)


- Authentification : (C7: Implement Digital Identity)[https://owasp.org/www-project-proactive-controls/v4/en/c7-implement-digital-identity.html]
  - 3 niveaux possibles 
    - L1 : mot de passe, 
    - L2 MFA,
    - L3 par certificat.
- Renforcement du navigateur: [C8: Help the Browser defend its User](https://owasp.org/www-project-proactive-controls/v4/en/c8-help-the-browser-defend-the-user.html)
Le renforcement est réalisé via les header ou des tag HTML (meta)
  - Prévenir les divulgations d'inforamations :
    - Activer HSTS : HTTP Strict Transport Security (pas de connexion http)
    - Activer les CSP
    - Mettre en place une Referrer-Policy pour éviter de transmettre des informations à des sites extérieurs.
- Clickjacking
  - X-Frame-Options (XFO)
  - CSP
- CSRF 
  - Utliser des cookies "Same-Origin"

## <img width="20px" src="../../../assets/images/prerequisites.svg" alt="target"> Prérequis 
- Matrice des permissions/rôles
- Dictionnaire des données classifié par niveau de criticité.


## <img width="20px" src="../../../assets/images/target.svg" alt="target"> Actions 
 1. Utiliser un contrôle de entrées coté client.
 2. Utiliser un contrôle des entrés utilisateur coté serveur:
    1. Utiliser une deny list : exemple rechercher la chaine <SCRIPT>
    2. Utiliser une allow list : regles de validation syntaxique et sémantique.
 3. Mass Assignment : Désactiver le binding automatique. Utiliser des Value Objects.
 4. Activer HSTS
 5. XSS (Cross-Site Scripting)
    1. Marquer les cookies sensibles en httpOnly pour que javascript ne puisse y accéder.
    2. Content Security Policy CSP : reduire la surface d'attaque. [TODO] déterminer les bonne CSP.
    [TODO] définir les CSP : CSP de base & CSP spécifiques ?
6. activer le secure flag pour les cookies pour bloquer leur transmission en clair.
7. Marquer les cookies SameSite.
 8. Eviter les mécanismes de sérialisation / désérialisation autant que possible et préférer un format de type JSON. Si impossble reprendre les préconisations OWASP.
 9. Utiliser des "Secure Architecture Patterns".
 10. Vérifier les dépendances : dépendances inutiles et failles de sécurités (CI).
 11. Mettre à jour les dépendances de façon pro active.
 12. Mettre en place/utiliser la Referrer-Policy.
 13. Utiliser [Trusted Types API](https://developer.mozilla.org/en-US/docs/Web/API/Trusted_Types_API).
 14. Utiliser le header X-Frame-Options et vérifier qu'il n'est présent qu'une seule fois.

