---
layout: page
title: POC VueFlow
permalink: /vue-flow-poc/
---

_Last updated: 2025-12-05_

## Table des matières

- [Ressources](#ressources-)
- [Poc VueFlow](#poc-vueflow-)
  - [Module Bâtir mon projet > Construire les trajectoires futures](#module-bâtir-mon-projet--construire-les-trajectoires-futures-)
    - [High Priority](#high-priority-)
      - [Accéder aux éléments existants](#accéder-aux-éléments-existants-)
      - [Importer ces éléments dans la zone de construction](#importer-ces-éléments-dans-la-zone-de-construction-)
      - [Créer et manipuler du contenu libre](#créer-et-manipuler-du-contenu-libre-)
      - [Gérer les connexions](#gérer-les-connexions-)
      - [Modifier et gérer les encarts](#modifier-et-gérer-les-encarts-)
    - [Low Priority](#low-priority-)
      - [Mettre une trajectoire en favoris](#mettre-une-trajectoire-en-favoris-)
      - [Différencier visuellement les encarts](#différencier-visuellement-les-encarts-)
      - [Exporter une trajectoire](#exporter-une-trajectoire-)
      - [Créer un aperçu visuel global](#créer-un-aperçu-visuel-global-)
      - [Partager une trajectoire à un tiers](#partager-une-trajectoire-à-un-tiers-)
  - [Carte mentale](#carte-mentale-)
    - [Fonctionnalités must have](#fonctionnalités-must-have-)
      - [Ajout d'Éléments Blocs Composants du Projet de Vie](#ajout-déléments-blocs-composants-du-projet-de-vie-)
      - [Déplacement des Éléments Blocs Composants](#déplacement-des-éléments-blocs-composants-)
      - [Suppression des Éléments Blocs Composants](#suppression-des-éléments-blocs-composants-)
      - [Création de Nouvelles Catégories Parentes](#création-de-nouvelles-catégories-parentes-)
      - [Création de Nœuds Enfants et Frères](#création-de-nœuds-enfants-et-frères-)
      - [Exportation](#exportation-)
      - [Zoom et Navigation](#zoom-et-navigation-)
    - [Fonctionnalités should have](#fonctionnalités-should-have-)
      - [Création de Nœuds (ou Sujets) nouveaux](#création-de-nœuds-ou-sujets-nouveaux-)
      - [Connexion et Relations](#connexion-et-relations-)
      - [Notes et Pièces Jointes](#notes-et-pièces-jointes-)
      - [Pliage/Dépliage des Branches](#pliagedépliage-des-branches-)
      - [Délimiteurs et Groupements](#délimiteurs-et-groupements-)
      - [Pouvoir partager sa carte mentale](#pouvoir-partager-sa-carte-mentale-)
      - [Enregistrement multiple](#enregistrement-multiple-)


## Ressources [⇧](#table-des-matières)

- [vueflow.dev](https://vueflow.dev/guide/){:target="_blank"}
- [Doc VueFlow](../vue-flow){:target="_blank"}


## Poc VueFlow [⇧](#table-des-matières)

### Module Bâtir mon projet > Construire les trajectoires futures [⇧](#table-des-matières)

#### High Priority [⇧](#table-des-matières)

Il s'agit ici des fonctionnalités essentielles à la construction et manipulation.

##### Accéder aux éléments existants [⇧](#table-des-matières)

- Besoin : Accéder aux éléments existants du profil (Toutes mes compétences, Mon parcours, Bâtir mon projet) :
  - Compétences APC / autres
  - Badges
  - Formations
  - Expériences
  - Rubrique Me connaître
  - Fiches métiers / mobilité

- Tests avec VueFlow : TODO

##### Importer ces éléments dans la zone de construction [⇧](#table-des-matières)

- Besoin : Importer ces éléments dans la zone de construction :
  - Via drag & drop
  - Ou, à défaut, via une action d’import

- Tests avec VueFlow : TODO

##### Créer et manipuler du contenu libre [⇧](#table-des-matières)

- Besoin : Créer et manipuler du contenu libre :
  - Blocs textuels
  - Liens cliquables
  - Encarts déplaçables librement

- Tests avec VueFlow : TODO

##### Gérer les connexions [⇧](#table-des-matières)

- Besoin : Gérer les connexions :
  - Création de connexions directes entre éléments
  - Création de sous-connexions
  - Connexions entre trajectoires

- Tests avec VueFlow : TODO

##### Modifier et gérer les encarts [⇧](#table-des-matières)

- Besoin : Modifier et gérer les encarts :
  - Modification des éléments importés depuis la bibliothèque
  - Suppression rapide d’un encart ou d’une connexion
  - Duplication d’un encart

- Tests avec VueFlow : TODO

#### Low Priority [⇧](#table-des-matières)

Il s'agit ici des fonctionnalités de confort et enrichissement.

##### Mettre une trajectoire en favoris [⇧](#table-des-matières)

- Besoin : Mettre une trajectoire en favoris

- Tests avec VueFlow : TODO

##### Différencier visuellement les encarts [⇧](#table-des-matières)

- Besoin : Différencier visuellement les encarts pour catégorisation

- Tests avec VueFlow : TODO

##### Exporter une trajectoire [⇧](#table-des-matières)

- Besoin : Exporter une trajectoire en PDF/PNG

- Tests avec VueFlow : TODO

##### Créer un aperçu visuel global [⇧](#table-des-matières)

- Besoin : Créer un aperçu visuel global de la trajectoire

- Tests avec VueFlow : TODO

##### Partager une trajectoire à un tiers [⇧](#table-des-matières)

- Besoin : Partager une trajectoire à un tiers

- Tests avec VueFlow : TODO

### Carte Mentale [⇧](#table-des-matières)

#### Fonctionnalités must have [⇧](#table-des-matières)

##### Ajout d'Éléments Blocs Composants du Projet de Vie [⇧](#table-des-matières)

- Besoin : Permettre à l'utilisateur de créer de nouveaux nœuds (blocs) avec un type ou un modèle prédéfini (ex: intérêt, motivation, trajectoire, fiche futur, expérience) pour représenter un composant du projet de vie qu’il va chercher dans une zone de recherche par auto-complétion.

- Tests avec VueFlow : Il est possible de créer de nouveaux blocs. En l'état les blocs sont automatiquement ajoutés, liés et typés en fonction du bloc `+` appuyé. Cependant, il est tout à fait envisageable d'avoir un bouton d'ajout de bloc dans lequel on choisira un type existant pour être placé librement et lié librement.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_mindmap_add_blocks.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>POC VueFlow - Carte mentale : Ajout de blocs</figcaption>
</figure>
<br />

##### Déplacement des Éléments Blocs Composants [⇧](#table-des-matières)

- Besoin : L'utilisateur doit pouvoir glisser-déposer n'importe quel bloc composant pour le repositionner visuellement sur la carte et le rattacher à une autre catégorie parente.

- Tests avec VueFlow : Le glissé-déposé des blocs est natif à VueFlow. Il est donc possible de déplacer des blocs. Si des relations de parent-enfant ont été définies, glisser-déposer un bloc parent fera glisser-déposer les blocs enfants de la même manière. Attention, ce lien parent-enfant n'a rien à voir avec les liens créés entre deux points d'encrage de blocs. C'est une relation écrite dans la définition du noeud.
TODO: De nouveaux tests sont nécessaires pour déterminer s'il est possible de créer des liens parent-enfant en temps réel pour des blocs libres voire même de supprimer des liens parent-enfant existant.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_mindmap_move_blocks.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>POC VueFlow - Carte mentale : Déplacement de blocs</figcaption>
</figure>
<br />

##### Suppression des Éléments Blocs Composants [⇧](#table-des-matières)

- Besoin : méthode simple (ex: clic droit, touche Suppr) pour supprimer définitivement un bloc ainsi que tous ses éventuels sous-éléments (nœuds enfants).

- Tests avec VueFlow : Les blocs sont supprimables sans problème. Dans une première version, j'ai ajouté un bouton de suppression sur chacun des blocs mais il est possible d'envisager une autre solution.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_mindmap_remove_blocks.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>POC VueFlow - Carte mentale : Suppression de blocs</figcaption>
</figure>
<br />

##### Création de Nouvelles Catégories Parentes [⇧](#table-des-matières)

- Besoin : pouvoir créer de nouveaux nœuds principaux ou de premier niveau (les "catégories parentes", ex: Profil, Mes centres d’intérêt) pour organiser les blocs composants.

- Tests avec VueFlow : J'ai placé quelques noeuds principaux (qui je suis, mes recherches, où je vais) autour du noeud racine (avec la photo de l'utilisateur). J'ai également associé au clic sur le bouton d'ajout d'une catégorie dans "Qui je suis" l'ouverture d'une modale permettant d'ajouter une ou plusieurs catégories sélectionnables. Il est donc tout à fait envisageable de faire cela à partir d'un bouton autour du noeud racine pour ajouter de nouveaux noeuds principaux.

##### Création de Nœuds Enfants et Frères [⇧](#table-des-matières)

- Besoin : Assurer la possibilité de relier des idées de manière hiérarchique (enfants) ou latérale (frères) au nœud principal.

- Tests avec VueFlow : Il est possible d'ajouter des liens de tout niveau (hiérarchique ou latéral). Sémantiquement à VueFlow, cela n'induit pas de relation parent-enfant ou fratrie. De plus, la notion de fratrie n'existe pas nativement en VueFlow. Mais l'utilisation de `data` peut permettre d'ajouter cette notion au besoin.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_mindmap_add_edges.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>POC VueFlow - Carte mentale : Ajout de liens entre les blocs</figcaption>
</figure>
<br />

##### Exportation [⇧](#table-des-matières)

- Besoin : Permettre l'exportation dans différents formats standards (ex: Image (PNG, JPG), Document (PDF)

- Tests avec VueFlow : L'exportation en image est réalisable et implémentée. Des tests supplémentaires sont nécessaires pour déterminer la possbilité d'exportation en document PDF.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_mindmap_screenshot.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>POC VueFlow - Carte mentale : Export PNG</figcaption>
</figure>
<br />

##### Zoom et Navigation [⇧](#table-des-matières)

- Besoin : Assurer des fonctions de zoom fluide et de navigation aisée avec un enregistrement automatique

- Tests avec VueFlow : Le pan and zoom est natif à VueFlow, cette fonctionnalité est donc par défaut implémentée. L'enregistrement est actuellement conditionné à l'appui sur un bouton spécifique mais il est possible de l'automatiser à certaines actions. Le pan and zoom actuel n'est pas sauvegardé, seulement l'état du graphe. Il est possible de sauvegardé le pan and zoom actuel mais ça rajoutera des informations dans le localStorage.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_mindmap_pan_and_zoom.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>POC VueFlow - Carte mentale : Zoom et navigation</figcaption>
</figure>
<br />

#### Fonctionnalités should have [⇧](#table-des-matières)

##### Création de Nœuds (ou Sujets) nouveaux [⇧](#table-des-matières)

- Besoin : Permettre l'ajout rapide de nouveaux sujets ou idées libres -> Chemin inverse : type les nouveaux sujets pour qu’ils soient automatiquement ajoutés dans la page “Me connaître”.

- Tests avec VueFlow : Non testé mais doit-être possible avec une mutation associée à l'ajout de noeud.
TODO: À tester.

##### Connexion et Relations [⇧](#table-des-matières)

- Besoin : Autoriser l'ajout de lignes de connexion non-hiérarchiques entre des nœuds de différentes branches pour montrer des liens conceptuels

- Tests avec VueFlow : On peut ajouter autant de liens que l'on souhaite. Le plus compliqué sera de les typer ou de les contraindre. Je pense qu'avec un sélecteur il est possible de surcharger la méthode onConnect pour créer des liens particuliers.
TODO : À tester.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_mindmap_add_edges.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>POC VueFlow - Carte mentale : Ajout de liens entre les blocs</figcaption>
</figure>
<br />

##### Notes et Pièces Jointes [⇧](#table-des-matières)

- Besoin : Permettre d'ajouter des notes détaillées aux nœuds sans surcharger la vue principale, ainsi que des liens hypertextes ou des pièces jointes (fichiers).

- Tests avec VueFlow : J'ai créé un noeud qui affiche une `Avcard` collapsible avec un `AvInput` dans le slot `title` et un `AvInput` en `textarea` dans le slot `default`. Cela permet d'ajouter un noeud de champ texte libre avec un titre et une corps et de masquer le corps au besoin. J'ai fait de même pour les noeuds affichant des liens : une `AvCard` collapsible, le lien cliquable dans le `title` et un `AvInput` dns le `default` pour renseigner le lien.
Pour ce qui est des pièces jointes, encore une fois en théorie on peut utiliser tout composant DSAV donc il devrait être possible d'intégrer un `AvFileUpload`. J'ai cependant peur de la lourdeur ainsi induite.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_mindmap_text_input.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>POC VueFlow - Carte mentale : Ajout d'une note détaillée</figcaption>
</figure>
<br />

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_mindmap_link_input.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>POC VueFlow - Carte mentale : Ajout d'un lien hypertexte</figcaption>
</figure>
<br />

##### Pliage/Dépliage des Branches [⇧](#table-des-matières)

- Besoin : Permettre de masquer ou d'afficher les sous-nœuds pour gérer la complexité et se concentrer sur certaines parties de la carte.

- Tests avec VueFlow : VueFlow propose une propriété `hidden` sur les noeuds et les liens. En l'exploitant et avec de la récursivité, il est possible de masque les enfants d'un noeud à qui on passerait une propriété `collapsed`. Il est aussi possible d'utiliser cette propriété pour ajouter la classe `av-sr-only` à certains éléments propre au noeud que l'on voudrait également masquer.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_mindmap_collapse_nodes.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>POC VueFlow - Carte mentale : Pliage et dépliage des branches</figcaption>
</figure>
<br />

##### Délimiteurs et Groupements [⇧](#table-des-matières)

- Besoin : Offrir une fonction pour encadrer ou grouper plusieurs nœuds ensemble (ex: un nuage ou une forme) pour indiquer une relation ou un concept commun.

- Tests avec VueFlow : TODO

##### Pouvoir partager sa carte mentale [⇧](#table-des-matières)

- Besoin : Pouvoir partager sa carte mentale (lien ?)

- Tests avec VueFlow : Il est possible d'enregistrer l'image de la carte mentale, cette image est partageable. Pour un partage par lien, je ne suis pas convaincu. Étant donné que beaucoup de données dépendront de l'utilisateur (données API), je ne suis pas sûr qu'il soit possible de partager un lien vers la carte mentale. Le JSON est partageable mais s'il nécessite de récupérer des données API (non testé actuellement avec de vraies données API), je pense que ça va coincer.

##### Enregistrement multiple [⇧](#table-des-matières)

- Besoin : Pouvoir créer plusieurs cartes mentales (jusqu’à 5 max ?) / Enregistrements et archivage de 5 cartes mentales

- Tests avec VueFlow : Dans le cadre du POC, je sauvegarde dans le `localStorage` les noeuds dans une variable `mind-map-flow-nodes` et les liens dans une variables `mind-map-flow-edges`. Pour les sauvegardes multiples, j'ai rajouté un index `1` / `2` pour tester, ça fonctionne. Il faudrait voir cependant si ce n'est pas trop lourd pour le `localStorage` des utilisateurs.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_mindmap_multiple_saves.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>POC VueFlow - Carte mentale : Enregistrement multiple</figcaption>
</figure>
<br />