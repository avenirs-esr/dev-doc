---
layout: page
title: Architecture - Frontend
permalink: /arch-front/
---

# Définition de la stack technique :
* **~~Utilisation d'un store : Vuex.~~**
* **Mise en place de la configuration (.env, NODE_ENV, intégration dans srv-dev avec Docker ?...) _DEADLINE DE DÉCISION 01/04/2025_**
* **Data fetching : ex Vue Query : pour la mise en cache, l'interaction avec la partie back en SSE et les stratégies de fallback (shortpooling), les optimisations (détection d'inactivité), etc. _DEADLINE DE DÉCISION 15/04/2025_**
* **Logs**

# Architecture:

* **Gestion des erreurs, avoir un mécanisme unifié, définir les formats d'échanges, etc.**
* **Test, unitaires, e2e, ou autre ?**
* **Internationalisation.**

* Par feature, définir une organisation de répertoire/fichiers pour qu'on puis passer d'un dev à l'autre et s'yn retrouver directement.
* Composants transverses (comment on les organise, comment on les partage, ou on les documente, etc)

* Qu'est-ce que l'on documente, comment et où (dans le code, dans un wiki, les deux ?)

* Accessibilité.

* CI/CD : ça pourra être intéressant d'en profiter pour commencer à mettre en place certains éléments au niveau de CI pas trop tard.

* Router : creuser les possibilités du routeur vuejs, par exemple lazy loading, les guards pour les routes (en lien avec l'authentification), SSR ?

* Paramétrage : commun entre les composants UI et la partie back (ex endpoints, les limites, les codes d'erreur, etc). Stratégie à définir sachant qu'on est sur des stacks techniques variables côté back (Java, typescript, etc).

* Modélisation : si on part du front pour déterminer ce dont il y a besoin pour constituer petit à petit le modèle, il faut mettre en place une façon de faire pour agréger et centraliser les infos.

* Mesure de perf et optimisation. Ca je pense que c'est bien de le voir pas trop tard, ca va permettre de valider des choix d'archi ou technique et de moins naviguer à vue.

* Bonnes pratiques diverses.