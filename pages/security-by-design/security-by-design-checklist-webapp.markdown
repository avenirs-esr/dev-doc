---
layout: page
title: Security/Privacy by design - Checklist webapp
permalink: /security-by-design-checklist-webapp/
page_content_classes: table-container
---
<div style="border: 2px dashed #e67e22; background: #fcf3e6; padding: 1em; margin: 1em 0; font-weight: bold; color: #e67e22;">
  ⚠️ Cette section est en cours de construction.
</div>
**Module :** Avenirs-ESR / ePortfolio - webbapps check list<br/> 
**Objet :** Check list de sécurité pour les applications web<br/>

**Révision :** 1.0.0.<br/>
**Date :** 10/04/2024.<br/>
**Auteur :** A. Deman.<br/>
**Commentaire :** Version initiale - Draft

-----

## Sources
[Top 10 Web Application Security Risks.](https://owasp.org/www-project-top-ten/)

## Points de vérification

- Contrôle d'accès :  (A01:2021-Broken Access Control)[https://owasp.org/Top10/A01_2021-Broken_Access_Control/]
  1.  Respecter le principe de moindre privilège par défaut (e.g. : pas d'accès admin pour un utilisateur lambda)
  2.  Utiliser une clé d'API pour les requête anonymes.
  3. Utiliser l’accès token pour toutes les autres requêtes, à chaque requête.
  4. Utiliser le composant d'access control (AC) [TODO] Déterminer composant
  (gère également les aspects CORS ?)
  5. Configuration CORS : n'autoriser que ce qui est nécessaire (si non géré par le lAC)
  6. Configurer les Content security Policy CSP 
  7. Utilisation de l'APIM => **checklist pour APIM (Logging, rate limit, etc.)**[TODO] APIM checklist
  8. Invalider les sessions (statefull) [TODO] Déterminer nos cas d'usage, s'il y en a.
  9. Utiliser des sessions stateless avec une fenêtre courte [TODO] déterminer la valeur
   
- Chiffrement : (A02:2021-Cryptographic Failures) [https://owasp.org/Top10/A02_2021-Cryptographic_Failures/]
  1. Se référer / Mettre à jour le dictionnaire des données [TODO] Dictionnaire des données avec classification.
  1. Toutes les communications sont chiffrées
  2. Utilisation d’algorithmes de chiffrement performants et récents: [TODO] Liste des algos à déterminer. Plus système que dev. 
  3. Ne pas stocker les données sensible sans nécessité.
  4. Ne pas mettre en cache les données sensibles.
  5. [TODO] voir comment les autres points qui concerne la cryptographie peuvent/doivent ou non être intégrés (e.g.: use of authenticated encryption).

- A03:2021-Injection [TODO]
- A05:2021-Security Misconfiguration [TODO]
- Composants : (A06:2021-Vulnerable and Outdated Components)[https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/]
  1. Supprimmer les dépendance inutiles [TODO] 
      - Déterminer les modes opératoires utilisables
      - Intégrer directement dans le pipeline de CI quand c'est possible [TODO] selectionner les outils (OWASP Dependency Check, retire.js, bot github, etc.)
      - Sourcer les librairies : uniquement des sources officielles, maintenues, etc. Utiliser des packages signés lorsque c'est possible.
      - Effectuer **un check de securité dans le pipeline CI** [TODO] Déterminer les outils.
- 
- A07:2021-Identification and Authentication Failures [TODO] 
- A08:2021-Software and Data Integrity Failures[TODO]
- A09:2021-Security Logging and Monitoring Failures[TODO]
- A10:2021-Server-Side Request Forgery[TODO] 
