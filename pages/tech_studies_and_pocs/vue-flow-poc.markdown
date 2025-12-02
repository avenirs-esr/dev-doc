---
layout: page
title: POC VueFlow
permalink: /vue-flow-poc/
---

_Last updated: 2025-12-11_

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
    - [Implémentation technique de la carte mentale](#implémentation-technique-de-la-carte-mentale-)
      - [MindMap.vue](#mindmapvue-)
      - [use-mind-map-flow.ts](#use-mind-map-flowts-)
      - [UserNode.vue](#usernodevue-)
      - [MainSectionNode.vue](#mainsectionnodevue-)
      - [SelfKnowledgeCategoyNode.vue](#selfknowledgecategoynodevue-)
      - [AddSelfKnowledgeCategoryButtonNode.vue](#addselfknowledgecategorybuttonnodevue-)
      - [AddSelfKnowledgeElementButtonNode.vue](#addselfknowledgeelementbuttonnodevue-)
      - [SelfKnowledgeElementNode.vue](#selfknowledgeelementnodevue-)
      - [TrajectoryNode.vue](#trajectorynodevue-)
      - [AddTrajectoryButtonNode.vue](#addtrajectorybuttonnodevue-)
      - [ResearchNode.vue](#researchnodevue-)
      - [AddResearchButtonNode.vue](#addresearchbuttonnodevue-)
      - [TextInputNode.vue](#textinputnodevue-)
      - [LinkInputNode.vue](#linkinputnodevue-)


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

#### Implémentation technique de la carte mentale [⇧](#table-des-matières)

##### MindMap.vue [⇧](#table-des-matières)

```js
// MindMap.vue

<script setup lang="ts">
import { GLOBAL_NODE_TYPES } from '@/common/components/VueFlow/global-nodes.types'
import { useEdges } from '@/common/composables/VueFlow/use-edges/use-edges'
import { useFlowScreenshot } from '@/common/composables/VueFlow/use-flow-screenshot/use-flow-screenshot'
import { useNodes } from '@/common/composables/VueFlow/use-nodes/use-nodes'
import ButtonNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/components/ButtonNode.vue'
import LinkInputNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/components/LinkInputNode.vue'
import MainSectionNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/components/MainSectionNode.vue'
import TextInputNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/components/TextInputNode.vue'
import UserNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/components/UserNode.vue'
import { useMindMapFlow } from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/composables/use-mind-map-flow'
import AddResearchButtonNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/researchs/components/AddResearchButtonNode/AddResearchButtonNode.vue'
import ResearchNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/researchs/components/ResearchNode/ResearchNode.vue'
import AddSelfKnowledgeCategoryButtonNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/self-knowledge/components/AddSelfKnowledgeCategoryButtonNode/AddSelfKnowledgeCategoryButtonNode.vue'
import AddSelfKnowledgeElementButtonNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/self-knowledge/components/AddSelfKnowledgeElementButtonNode/AddSelfKnowledgeElementButtonNode.vue'
import SelfKnowledgeCategoryNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/self-knowledge/components/SelfKnowledgeCategoryNode/SelfKnowledgeCategoryNode.vue'
import SelfKnowledgeElementNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/self-knowledge/components/SelfKnowledgeElementNode/SelfKnowledgeElementNode.vue'
import AddTrajectoryButtonNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/trajectories/components/AddTrajectoryButtonNode/AddTrajectoryButtonNode.vue'
import TrajectoryNode from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/trajectories/components/TrajectoryNode/TrajectoryNode.vue'
import { AvButton, AvIconText, MDI_ICONS, RI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { useVueFlow, VueFlow } from '@vue-flow/core'
import { useI18n } from 'vue-i18n'

const { t } = useI18n()
const {
  saveCurrentState,
  restoreSavedState,
  resetToInitialState,
} = useMindMapFlow()
const { doScreenshot } = useFlowScreenshot()
const { addNode } = useNodes()
const { onConnect } = useEdges()

const { nodes, edges } = useVueFlow()

restoreSavedState('mind-map', '1')

function addTextInputNode () {
  addNode({
    type: GLOBAL_NODE_TYPES.TEXT_INPUT,
    data: { title: '', content: '' },
  })
}

function addLinkInputNode () {
  addNode({
    type: GLOBAL_NODE_TYPES.LINK_INPUT,
    data: { link: '' },
  })
}
</script>

<template>
  <div class="av-flex-col-sm">
    <AvIconText
      :icon="RI_ICONS.LOADER_LINE"
      icon-color="var(--icon)"
      text="Ma carte mentale"
      text-color="var(--title)"
      typography-class="n5"
      gap="var(--spacing-xs)"
    />
    <div class="av-row av-row--middle av-row--between av-flex-row-sm">
      <AvButton
        :label="t('student.buildProject.mindMap.actions.addFreeText')"
        :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
        @click="addTextInputNode"
      />
      <AvButton
        :label="t('student.buildProject.mindMap.actions.addHyperlink')"
        :icon="MDI_ICONS.LINK"
        @click="addLinkInputNode"
      />
      <AvButton
        :label="t('global.buttons.save')"
        :icon="MDI_ICONS.CONTENT_SAVE_OUTLINE"
        @click="() => saveCurrentState('mind-map', '1')"
      />
      <AvButton
        :label="t('global.buttons.restore')"
        :icon="MDI_ICONS.SWAP_HORIZONTAL"
        @click="() => restoreSavedState('mind-map', '1')"
      />
      <AvButton
        :label="t('global.buttons.reset')"
        :icon="RI_ICONS.LOADER_LINE"
        @click="resetToInitialState"
      />
      <AvButton
        :label="t('student.buildProject.mindMap.actions.screenshot')"
        :icon="MDI_ICONS.FILE_IMAGE_OUTLINE"
        @click="() => doScreenshot('mind-map')"
      />
    </div>
    <div class="poc-container">
      <VueFlow
        :nodes="nodes"
        :edges="edges"
        @connect="onConnect"
      >
        <!-- bind your custom node type to a component by using slots, slot names are always `node-<type>` -->
        <template #node-user="userNodeProps">
          <UserNode v-bind="userNodeProps" />
        </template>
        <template #node-main-section="mainSectionNodeProps">
          <MainSectionNode v-bind="mainSectionNodeProps" />
        </template>
        <template #node-button="buttonNodeProps">
          <ButtonNode v-bind="buttonNodeProps" />
        </template>
        <template #node-self-knowledge-category="selfKnowledgeCategoryNodeProps">
          <SelfKnowledgeCategoryNode v-bind="selfKnowledgeCategoryNodeProps" />
        </template>
        <template #node-add-self-knowledge-category-button="addSelfKnowledgeCategoryButtonNodeProps">
          <AddSelfKnowledgeCategoryButtonNode v-bind="addSelfKnowledgeCategoryButtonNodeProps" />
        </template>
        <template #node-add-self-knowledge-element-button="addSelfKnowledgeElementButtonNodeProps">
          <AddSelfKnowledgeElementButtonNode v-bind="addSelfKnowledgeElementButtonNodeProps" />
        </template>
        <template #node-self-knowledge-element="selfKnowledgeElementNodeProps">
          <SelfKnowledgeElementNode v-bind="selfKnowledgeElementNodeProps" />
        </template>
        <template #node-trajectory="trajectoryNodeProps">
          <TrajectoryNode v-bind="trajectoryNodeProps" />
        </template>
        <template #node-add-trajectory-button="addTrajectoryButtonNodeProps">
          <AddTrajectoryButtonNode v-bind="addTrajectoryButtonNodeProps" />
        </template>
        <template #node-research="researchNodeProps">
          <ResearchNode v-bind="researchNodeProps" />
        </template>
        <template #node-add-research-button="addResearchButtonNodeProps">
          <AddResearchButtonNode v-bind="addResearchButtonNodeProps" />
        </template>
        <template #node-text-input="textInputNodeProps">
          <TextInputNode v-bind="textInputNodeProps" />
        </template>
        <template #node-link-input="linkInputNodeProps">
          <LinkInputNode v-bind="linkInputNodeProps" />
        </template>

        <!-- bind your custom edge type to a component by using slots, slot names are always `edge-<type>` -->
      </VueFlow>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.poc-container {
  border: 1px solid var(--stroke);
  height: 80vh;
  width: 100%;
}

:deep(.vue-flow__pane) {
  background-color: var(--other-background-base);
}

:deep(.vue-flow__handle) {
  &.source {
    background: var(--dark-background-primary1);
    border-color: var(--other-background-base);
  }
}
</style>
```

##### use-mind-map-flow.ts [⇧](#table-des-matières)

```js
// use-mind-map-flow.ts

import type { Edge, Node } from '@vue-flow/core'
import { GLOBAL_NODE_HANDLES } from '@/common/components/VueFlow/global-nodes.types'
import { useFlowState } from '@/common/composables/VueFlow/use-flow-state/use-flow-state'
import { getEdgeId, remToPx } from '@/common/utils/vue-flow/vue-flow'
import { RESEARCHS_NODE_TYPES } from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/researchs/researchs-nodes.types'
import { SELF_KNOWLEDGE_NODE_TYPES } from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/self-knowledge/self-knowledge-nodes.types'
import { TRAJECTORIES_NODE_TYPES } from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/trajectories/trajectories-nodes.types'
import { MIND_MAP_NODE_TYPES } from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/mind-map-nodes.types'
import { MDI_ICONS } from '@avenirs-esr/avenirs-dsav'

export interface UseMindMapFlowReturn {
  saveCurrentState: (prefix: string, index: string) => void
  restoreSavedState: (prefix: string, index: string) => void
  resetToInitialState: () => void
}

export function useMindMapFlow (): UseMindMapFlowReturn {
  // === User initial nodes definitions ===
  const userNode: Node = {
    id: 'user',
    type: MIND_MAP_NODE_TYPES.USER,
    position: { x: 700, y: 50 },
    draggable: true,
    data: {
      width: '5rem',
      height: '5rem',
    }
  }

  // === Self knowledge initial nodes definitions ===
  const selfKnowledgeNode: Node = {
    id: 'self-knowledge',
    type: MIND_MAP_NODE_TYPES.MAIN_SECTION,
    parentNode: userNode.id,
    position: { x: -230, y: 10 },
    data: { label: 'Qui je suis ?', right: true, left: true },
  }

  const addSelfKnowledgeButtonNode: Node = {
    id: 'add-self-knowledge',
    type: SELF_KNOWLEDGE_NODE_TYPES.ADD_SELF_KNOWLEDGE_CATEGORY_BUTTON,
    parentNode: selfKnowledgeNode.id,
    position: { x: -80, y: 5 },
    data: {
      label: 'Ajouter un élément',
      icon: MDI_ICONS.PLUS_CIRCLE_OUTLINE,
      right: true,
      left: true,
    },
  }

  // === Self knowledge initial edges definitions ===
  const initialSelfKnowledgeEdges: Edge[] = [
    {
      source: userNode.id,
      sourceHandle: GLOBAL_NODE_HANDLES.LEFT,
      target: selfKnowledgeNode.id,
      targetHandle: GLOBAL_NODE_HANDLES.RIGHT,
    },
    {
      source: selfKnowledgeNode.id,
      sourceHandle: GLOBAL_NODE_HANDLES.LEFT,
      target: addSelfKnowledgeButtonNode.id,
      targetHandle: GLOBAL_NODE_HANDLES.RIGHT,
    }
  ].map(edge => ({ ...edge, id: getEdgeId(edge), type: 'smoothstep' }))

  // === Researchs initial nodes definitions ===
  const researchsNode: Node = {
    id: 'researchs',
    type: MIND_MAP_NODE_TYPES.MAIN_SECTION,
    parentNode: userNode.id,
    position: { x: remToPx(5) + 30, y: 10 },
    data: { label: 'Mes recherches', left: true, right: true },
  }

  const addResearchButtonNode: Node = {
    id: 'add-research',
    type: RESEARCHS_NODE_TYPES.ADD_RESEARCH_BUTTON,
    parentNode: researchsNode.id,
    position: { x: researchsNode.data.label.length * 16 + 60, y: -2 },
    data: {
      label: 'Ajouter un élément',
      icon: MDI_ICONS.PLUS_CIRCLE_OUTLINE,
      left: true,
      right: true,
    },
  }

  // Researchs initial edges definitions
  const initialResearchsEdges: Edge[] = [
    {
      source: userNode.id,
      sourceHandle: GLOBAL_NODE_HANDLES.RIGHT,
      target: researchsNode.id,
      targetHandle: GLOBAL_NODE_HANDLES.LEFT,
    },
    {
      source: researchsNode.id,
      sourceHandle: GLOBAL_NODE_HANDLES.RIGHT,
      target: addResearchButtonNode.id,
      targetHandle: GLOBAL_NODE_HANDLES.LEFT,
    }
  ].map(edge => ({ ...edge, id: getEdgeId(edge), type: 'smoothstep' }))

  // === Trajectories initial nodes definitions ===
  const trajectoriesNode: Node = {
    id: 'trajectories',
    type: MIND_MAP_NODE_TYPES.MAIN_SECTION,
    parentNode: userNode.id,
    position: { x: -37, y: remToPx(5) + 40 },
    data: { label: 'Où je vais ?', top: true, bottom: true },
  }

  const addTrajectoryButtonNode: Node = {
    id: 'add-trajectory',
    type: TRAJECTORIES_NODE_TYPES.ADD_TRAJECTORY_BUTTON,
    parentNode: trajectoriesNode.id,
    position: { x: trajectoriesNode.data.label.length * 8 - 40, y: 100 },
    data: {
      label: 'Ajouter un élément',
      icon: MDI_ICONS.PLUS_CIRCLE_OUTLINE,
      top: true,
      bottom: true,
    },
  }

  // === Trajectories initial edges definitions ===
  const initialTrajectoriesEdges: Edge[] = [
    {
      source: userNode.id,
      sourceHandle: GLOBAL_NODE_HANDLES.BOTTOM,
      target: trajectoriesNode.id,
      targetHandle: GLOBAL_NODE_HANDLES.TOP,
    },
    {
      source: trajectoriesNode.id,
      sourceHandle: GLOBAL_NODE_HANDLES.BOTTOM,
      target: addTrajectoryButtonNode.id,
      targetHandle: GLOBAL_NODE_HANDLES.TOP,
    }
  ].map(edge => ({ ...edge, id: getEdgeId(edge), type: 'smoothstep' }))

  // === Initial nodes ===
  const initialNodes: Node[] = [
    userNode,
    selfKnowledgeNode,
    addSelfKnowledgeButtonNode,
    researchsNode,
    addResearchButtonNode,
    trajectoriesNode,
    addTrajectoryButtonNode
  ]

  // === Initial edges ===
  const initialEdges: Edge[] = [
    ...initialSelfKnowledgeEdges,
    ...initialResearchsEdges,
    ...initialTrajectoriesEdges,
  ].map(edge => ({ ...edge, id: getEdgeId(edge), type: 'smoothstep' }))

  const { saveCurrentState, restoreSavedState, resetToInitialState } = useFlowState({
    initialNodes,
    initialEdges,
  })

  return {
    saveCurrentState,
    restoreSavedState,
    resetToInitialState,
  }
}
```

##### UserNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import { GLOBAL_NODE_HANDLES } from '@/common/components/VueFlow/global-nodes.types'
import { useStudentSummaryQuery } from '@/features/student/user'
import { Handle, type NodeProps, Position } from '@vue-flow/core'

defineProps<NodeProps>()

const { data: studentData } = useStudentSummaryQuery()
const profilePictureUrl = computed(() => studentData.value?.profilePicture.url.replace('http://localhost:5173', 'https://qualif.avenirs-esr.fr'))

const handles = [
  { id: GLOBAL_NODE_HANDLES.TOP, position: Position.Top },
  { id: GLOBAL_NODE_HANDLES.RIGHT, position: Position.Right },
  { id: GLOBAL_NODE_HANDLES.BOTTOM, position: Position.Bottom },
  { id: GLOBAL_NODE_HANDLES.LEFT, position: Position.Left },
]
</script>

<template>
  <div class="vue-flow__node-default">
    <img
      :src="profilePictureUrl"
      alt="Photo de profil"
      class="user-picture"
    >

    <Handle
      v-for="handle in handles"
      :key="handle.id"
      v-bind="handle"
      type="source"
    />
  </div>
</template>

<style lang="scss" scoped>
.vue-flow__node-default {
  width: v-bind('data.width');
  height: v-bind('data.height');
  border-radius: var(--radius-xl);
  border: none;
  padding: var(--spacing-none);
  overflow: hidden;

  .user-picture {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
}
</style>
```

##### MainSectionNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import NodeTemplate from '@/common/components/VueFlow/NodeTemplate/NodeTemplate.vue'

defineProps<NodeProps>()
</script>

<template>
  <NodeTemplate
    v-bind="$props"
    title-only
    title-background="var(--other-background-base)"
  >
    <template #title>
      <span class="s1-bold">{{ data.label }}</span>
    </template>
  </NodeTemplate>
</template>

<style lang="scss" scoped>
.s1-bold {
  color: var(--text2);
}
</style>
```

##### SelfKnowledgeCategoyNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import NodeTemplate from '@/common/components/VueFlow/NodeTemplate/NodeTemplate.vue'
import { useSelfKnowledgeCategory } from '@/features/student/selfKnowledge/composables/use-self-knowledge-category/use-self-knowledge-category'
import { AvBadge } from '@avenirs-esr/avenirs-dsav'

const { id } = defineProps<NodeProps>()

const { category, categoryIcon } = useSelfKnowledgeCategory(id)
</script>

<template>
  <NodeTemplate
    v-bind="$props"
    title-only
    title-background="var(--other-background-base)"
  >
    <template #title>
      <AvBadge
        :label="category?.title ?? data.label"
        small
        color="var(--text1)"
        background-color="var(--surface-background)"
        border-color="var(--other-border-skill-card)"
        :icon="categoryIcon"
      />
    </template>
  </NodeTemplate>
</template>
```

##### AddSelfKnowledgeCategoryButtonNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import ButtonNodeTemplate from '@/common/components/VueFlow/ButtonNodeTemplate/ButtonNodeTemplate.vue'
import { GLOBAL_NODE_HANDLES } from '@/common/components/VueFlow/global-nodes.types'
import { useModal } from '@/common/composables'
import { useNodes } from '@/common/composables/VueFlow/use-nodes/use-nodes'
import { SELF_KNOWLEDGE_NODE_TYPES } from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/self-knowledge/self-knowledge-nodes.types'
import { useSelfKnowledgeCategoriesAvailableQuery, useSelfKnowledgeCategoriesQuery } from '@/features/student/selfKnowledge/queries/self-knowledge.query/self-knowledge.query'
import { AvCheckbox, AvCheckboxesGroup, AvModal, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { type NodeProps, useVueFlow } from '@vue-flow/core'
import { useI18n } from 'vue-i18n'

const { id } = defineProps<NodeProps>()

const { t } = useI18n()
const { showModal, displayModal, hideModal } = useModal()
const { nodes } = useVueFlow()
const { addNode } = useNodes()

const { categories } = useSelfKnowledgeCategoriesQuery()
const { categoriesAvailable } = useSelfKnowledgeCategoriesAvailableQuery()

const allCategories = computed(() => categories.value.concat(categoriesAvailable.value))
const availableCategories = computed(() => allCategories.value.filter(category => categoryExists(category.id) === false))
const usedCategoriesCount = computed(() => allCategories.value.length - availableCategories.value.length)

const selectedCategoriesIds = ref<string[]>([])

function closeModal () {
  selectedCategoriesIds.value = []
  hideModal()
}

function categoryExists (categoryId: string) {
  return nodes.value.some(node =>
    node.type === SELF_KNOWLEDGE_NODE_TYPES.SELF_KNOWLEDGE_CATEGORY && node.id === categoryId
  )
}

function onConfirmAddCategories () {
  // === Create new nodes for each selected category ===
  selectedCategoriesIds.value.forEach((selectedId) => {
    const category = allCategories.value.find(category => category.id === selectedId)

    if (!category) {
      throw new Error(`Category with id ${selectedId} not found`)
    }

    addNode({
      id: selectedId,
      type: SELF_KNOWLEDGE_NODE_TYPES.SELF_KNOWLEDGE_CATEGORY,
      position: {
        x: -150 - category.title.length * 7,
        y: (usedCategoriesCount.value) * 175 + 20,
      },
      data: {
        label: category.title,
        right: true,
        bottom: true
      },
      parentId: id,
      parentHandle: GLOBAL_NODE_HANDLES.LEFT,
      nodeHandle: GLOBAL_NODE_HANDLES.RIGHT,
    })

    addNode({
      id: `add-element-${selectedId}`,
      type: SELF_KNOWLEDGE_NODE_TYPES.ADD_SELF_KNOWLEDGE_ELEMENT_BUTTON,
      position: {
        x: category.title.length * 4,
        y: 80,
      },
      data: {
        categoryId: selectedId,
        top: true,
        bottom: true,
      },
      parentId: selectedId,
      parentHandle: GLOBAL_NODE_HANDLES.BOTTOM,
      nodeHandle: GLOBAL_NODE_HANDLES.TOP,
    })
  })

  closeModal()
}
</script>

<template>
  <ButtonNodeTemplate
    v-bind="$props"
    :label="t('student.buildProject.mindMap.selfKnowledge.addCategoryButton.label')"
    :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
    icon-only
    @click="displayModal"
  >
    <AvModal
      :opened="showModal"
      :close-button-label="t('global.buttons.cancel')"
      :confirm-button-label="t('student.buildProject.mindMap.selfKnowledge.addCategoryButton.confirm', { count: selectedCategoriesIds.length })"
      :confirm-button-icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
      :confirm-button-disabled="selectedCategoriesIds.length === 0"
      @close="closeModal"
      @confirm="onConfirmAddCategories"
    >
      <div
        v-if="availableCategories.length > 0"
        class="add-self-knowledge-categories-modal__body"
      >
        <AvCheckboxesGroup id="add-self-knowledge-categories-modal-checkboxes-group">
          <AvCheckbox
            v-for="category in availableCategories"
            :id="category.id"
            :key="category.id"
            v-model="selectedCategoriesIds"
            :value="category.id"
            :name="category.id"
            :label="category.title"
          />
        </AvCheckboxesGroup>
      </div>
    </AvModal>
  </ButtonNodeTemplate>
</template>
```

##### AddSelfKnowledgeElementButtonNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import ButtonNodeTemplate from '@/common/components/VueFlow/ButtonNodeTemplate/ButtonNodeTemplate.vue'
import { GLOBAL_NODE_HANDLES } from '@/common/components/VueFlow/global-nodes.types'
import { useModal } from '@/common/composables'
import { useNodes } from '@/common/composables/VueFlow/use-nodes/use-nodes'
import { type AddElementFormData, useAddElementForm } from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/self-knowledge/composables/use-add-element-form'
import { SELF_KNOWLEDGE_NODE_TYPES } from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/self-knowledge/self-knowledge-nodes.types'
import CategoryElementRatingRadioButtonSet from '@/features/student/selfKnowledge/components/interactions/inputs/CategoryElementRatingRadioButtonSet/CategoryElementRatingRadioButtonSet.vue'
import { useSelfKnowledgeCategoryElementsViewQuery } from '@/features/student/selfKnowledge/queries/self-knowledge.query/self-knowledge.query'
import { useToasterStore } from '@/store'
import { AvCheckbox, AvCheckboxesGroup, AvInput, AvModal, AvTab, AvTabs, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { markRaw } from 'vue'
import { useI18n } from 'vue-i18n'

const { id, data } = defineProps<NodeProps>()

const { showModal, displayModal, hideModal } = useModal()
const { addNode, findNodeByTitleAndDescription } = useNodes()
const { addErrorMessage } = useToasterStore()
const { t } = useI18n()

const { elements } = useSelfKnowledgeCategoryElementsViewQuery({
  selfKnowledgeCategoryId: computed(() => data.categoryId as string),
  page: ref(0), // temporary value for POC purpose - need a real pagination later
  pageSize: ref(12) // temporary value for POC purpose - need a real pagination later
})
const { form, isModified, isValid, resetForm } = useAddElementForm(data => onConfirmAddElements(data))
const FormField = markRaw(form.Field)

const availableElements = computed(() =>
  elements.value.filter((element) => {
    const existingNode = findNodeByTitleAndDescription(element.title, element.description)
    return !existingNode
  })
)
const usedElementsCount = computed(() =>
  elements.value.length - availableElements.value.length
)

enum TabIndex {
  ELEMENTS = 0,
  CUSTOM_ELEMENTS = 1,
}

const selectedElementsIds = ref<string[]>([])
const activeTab = ref(TabIndex.ELEMENTS)

function closeModal () {
  selectedElementsIds.value = []
  resetForm()
  hideModal()
}

function onConfirmAddElements (formData?: AddElementFormData) {
  const idsToAdd = formData ? [crypto.randomUUID()] : selectedElementsIds.value

  idsToAdd.forEach((selectedId) => {
    let title: string
    let description: string
    let rating: number | undefined

    if (activeTab.value === TabIndex.ELEMENTS) {
      const element = elements.value.find(element => element.id === selectedId)
      if (!element) {
        addErrorMessage(t('student.buildProject.mindMap.selfKnowledge.addElementButton.errors.elementNotFound', { id: selectedId }))
        throw new Error(t('student.buildProject.mindMap.selfKnowledge.addElementButton.errors.elementNotFound', { id: selectedId }))
      }
      title = element.title
      description = element.description
      rating = element.rating
    }
    else {
      if (!formData) {
        addErrorMessage(t('student.buildProject.mindMap.selfKnowledge.addElementButton.errors.formDataUndefined'))
        throw new Error(t('student.buildProject.mindMap.selfKnowledge.addElementButton.errors.formDataUndefined'))
      }
      title = formData.title
      description = formData.description
      rating = formData.rating
    }

    const existingNode = findNodeByTitleAndDescription(title, description)
    if (existingNode) {
      addErrorMessage(t('student.buildProject.mindMap.selfKnowledge.addElementButton.errors.existingNode'))
      throw new Error(t('student.buildProject.mindMap.selfKnowledge.addElementButton.errors.existingNode'))
    }

    addNode({
      id: selectedId,
      type: SELF_KNOWLEDGE_NODE_TYPES.SELF_KNOWLEDGE_ELEMENT,
      parentId: id,
      parentHandle: GLOBAL_NODE_HANDLES.BOTTOM,
      width: 300,
      position: {
        x: 60,
        y: (usedElementsCount.value) * 100 + 40,
      },
      data: {
        title,
        description,
        rating,
        categoryId: data.categoryId,
        left: true,
      },
    })
  })

  closeModal()
}
</script>

<template>
  <ButtonNodeTemplate
    v-bind="$props"
    :label="t('student.buildProject.mindMap.selfKnowledge.addElementButton.label')"
    :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
    icon-only
    small
    @click="displayModal"
  >
    <AvModal
      :opened="showModal"
      :close-button-label="t('global.buttons.cancel')"
      :confirm-button-label="t('student.buildProject.mindMap.selfKnowledge.addElementButton.confirm', { count: activeTab === TabIndex.ELEMENTS ? selectedElementsIds.length : 1 })"
      :confirm-button-icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
      :confirm-button-disabled="activeTab === TabIndex.ELEMENTS ? selectedElementsIds.length === 0 : !isModified || !isValid"
      @close="closeModal"
      @confirm="activeTab === TabIndex.ELEMENTS ? onConfirmAddElements() : form.handleSubmit()"
    >
      <AvTabs
        v-model="activeTab"
        compact
      >
        <AvTab
          :title="t('student.buildProject.mindMap.selfKnowledge.addElementButton.tabs.existing.title')"
          :icon="MDI_ICONS.BOOK_LOCATION_OUTLINE"
        >
          <div
            v-if="availableElements.length > 0"
            class="add-self-knowledge-categories-modal__body"
          >
            <AvCheckboxesGroup id="add-self-knowledge-categories-modal-checkboxes-group">
              <AvCheckbox
                v-for="element in availableElements"
                :id="element.id"
                :key="element.id"
                v-model="selectedElementsIds"
                :value="element.id"
                :name="element.id"
                :label="element.title"
              />
            </AvCheckboxesGroup>
          </div>
        </AvTab>

        <AvTab
          :title="t('student.buildProject.mindMap.selfKnowledge.addElementButton.tabs.new.title')"
          :icon="MDI_ICONS.STARS"
        >
          <form
            id="add-custom-element-form"
            @submit.prevent.stop="form.handleSubmit"
          >
            <div class="form">
              <FormField name="title">
                <template #default="{ field }">
                  <AvInput
                    v-model="field.state.value"
                    :error-message="field.state.meta.errors.join(', ')"
                    :label="t('student.buildProject.mindMap.selfKnowledge.addElementButton.tabs.new.form.title')"
                    required
                    @input="(e: Event) => field.handleChange((e.target as HTMLInputElement).value)"
                  />
                </template>
              </FormField>
              <FormField name="description">
                <template #default="{ field }">
                  <AvInput
                    v-model="field.state.value"
                    :error-message="field.state.meta.errors.join(', ')"
                    :label="t('student.buildProject.mindMap.selfKnowledge.addElementButton.tabs.new.form.description')"
                    is-textarea
                    required
                    @input="(e: Event) => field.handleChange((e.target as HTMLInputElement).value)"
                  />
                </template>
              </FormField>
              <FormField name="rating">
                <template #default="{ field }">
                  <CategoryElementRatingRadioButtonSet
                    :model-value="field.state.value ?? undefined"
                    :error-message="field.state.meta.errors?.join(', ')"
                    @blur="field.handleBlur"
                    @update:model-value="(value) => field.handleChange(value ?? 0)"
                  />
                </template>
              </FormField>
            </div>
          </form>
        </AvTab>
      </AvTabs>
    </AvModal>
  </ButtonNodeTemplate>
</template>
```

##### SelfKnowledgeElementNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import { EErrorCode } from '@/api/avenir-esr'
import { Rating } from '@/common/components'
import TitleDescriptionNodeTemplate from '@/common/components/VueFlow/TitleDescriptionNodeTemplate/TitleDescriptionNodeTemplate.vue'
import { useNodes } from '@/common/composables/VueFlow/use-nodes/use-nodes'
import { useAddSelfKnowledgeCategoryElementMutation, useUpdateSelfKnowledgeElementMutation } from '@/features/student/selfKnowledge/queries/self-knowledge.query/self-knowledge.query'
import { useToasterStore } from '@/store'
import { useI18n } from 'vue-i18n'

const { id, data } = defineProps<NodeProps>()

const { updateNodeId } = useNodes()

const { t } = useI18n()
const { addSuccessMessage, addErrorMessage } = useToasterStore()
const { mutate: addSelfKnowledgeCategoryElement } = useAddSelfKnowledgeCategoryElementMutation({
  onSuccess: (newElement) => {
    addSuccessMessage(t('student.buildProject.mindMap.selfKnowledge.element.success'))
    updateNodeId(id, newElement.id)
  },
  onError: error => addErrorMessage({
    title: t('student.buildProject.mindMap.selfKnowledge.element.error'),
    description: error.message,
  })
})
const { mutate: updateSelfKnowledgeElement } = useUpdateSelfKnowledgeElementMutation({
  onSuccess: () => addSuccessMessage(t('student.buildProject.mindMap.selfKnowledge.element.success')),
  onError: (error) => {
    if (isEErrorCode(error.code) && error.code === EErrorCode.SELF_KNOWLEDGE_ELEMENT_NOT_FOUND) {
      addSelfKnowledgeCategoryElement({
        selfKnowledgeCategoryId: data.categoryId as string,
        element: {
          title: data.title,
          description: data.description,
          rating: data.rating > 0 ? data.rating : undefined,
        },
      })
    }
    else {
      addErrorMessage({
        title: t('student.buildProject.mindMap.selfKnowledge.element.error'),
        description: error.message,
      })
    }
  }
})

function isEErrorCode (code: unknown): code is EErrorCode {
  return Object.values(EErrorCode).includes(code as EErrorCode)
}
</script>

<template>
  <TitleDescriptionNodeTemplate
    v-bind="$props"
    with-profile-update
    @update-in-profile="() => {
      updateSelfKnowledgeElement({
        selfKnowledgeElementId: id,
        element: {
          title: data.title,
          description: data.description,
          rating: data.rating > 0 ? data.rating : undefined,
        },
      })
    }"
  >
    <Rating
      v-if="data.rating"
      :rating="data.rating"
      :with-background="false"
    />
  </TitleDescriptionNodeTemplate>
</template>
```

##### TrajectoryNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import NodeTemplate from '@/common/components/VueFlow/NodeTemplate/NodeTemplate.vue'
import { AvIcon, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'

defineProps<NodeProps>()
</script>

<template>
  <NodeTemplate
    v-bind="$props"
    title-only
    title-background="transparent"
  >
    <template #title>
      <div class="av-row av-row--middle">
        <div class="av-row av-flex-row-xxs av-row--middle left-container">
          <div class="icon-container">
            <AvIcon
              :name="MDI_ICONS.ARROW_DECISION"
              color="var(--other-background-base)"
              :size="2"
            />
          </div>
          <div class="av-flex-col-xxs">
            <span class="caption-bold">{{ data.title }}</span>
            <span class="caption-bold">{{ data.subtitle }}</span>
          </div>
        </div>
        <div class="right-container">
          <span class="caption-regular">{{ data.description }}</span>
        </div>
      </div>
    </template>
  </NodeTemplate>
</template>

<style lang="scss" scoped>
.icon-container {
  background-color: var(--dark-background-primary1);
  border-radius: var(--radius-full);
  padding: var(--spacing-xxs)
}

.left-container {
  padding: var(--spacing-none) var(--spacing-xs);
  background-color: var(--surface-background);
  gap: var(--spacing-sm);
  height: var(--dimension-5xl);
  width: 11rem;
}

.right-container {
  display: flex;
  align-items: center;
  padding: var(--spacing-none) var(--spacing-xs);
}

:deep(.av-card__title) {
  padding: var(--spacing-none);
}
</style>
```

##### AddTrajectoryButtonNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import ButtonNodeTemplate from '@/common/components/VueFlow/ButtonNodeTemplate/ButtonNodeTemplate.vue'
import { GLOBAL_NODE_HANDLES } from '@/common/components/VueFlow/global-nodes.types'
import { useNodes } from '@/common/composables/VueFlow/use-nodes/use-nodes'
import { TRAJECTORIES_NODE_TYPES } from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/trajectories/trajectories-nodes.types'
import { MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { useI18n } from 'vue-i18n'

const { id } = defineProps<NodeProps>()

const { t } = useI18n()
const { addNode } = useNodes()

const trajectoryNodesCount = ref(0)

function addTrajectoryNode () {
  addNode({
    id: `trajectory-${crypto.randomUUID()}`,
    type: TRAJECTORIES_NODE_TYPES.TRAJECTORY,
    position: { x: 100, y: trajectoryNodesCount.value * 140 },
    data: {
      title: `Trajectoire ${trajectoryNodesCount.value + 1}`,
      subtitle: 'Sous titre',
      description: 'Description de la trajectoire',
      left: true,
    },
    parentId: id,
    parentHandle: GLOBAL_NODE_HANDLES.BOTTOM,
  })

  trajectoryNodesCount.value += 1
}
</script>

<template>
  <ButtonNodeTemplate
    v-bind="$props"
    :label="t('student.buildProject.mindMap.trajectories.addTrajectoryButton.label')"
    :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
    @click="addTrajectoryNode"
  />
</template>
```

##### ResearchNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import NodeTemplate from '@/common/components/VueFlow/NodeTemplate/NodeTemplate.vue'

defineProps<NodeProps>()
</script>

<template>
  <NodeTemplate
    v-bind="$props"
    border-color="var(--other-border-skill-card)"
    title-background="var(--surface-background)"
  >
    <template #title>
      <div class="av-row av-row--middle av-row--between av-flex-row-xs">
        <span class="b1-bold">{{ data.title }}</span>
      </div>
    </template>

    <div class="description-container">
      <span class="caption-regular">{{ data.description }}</span>
    </div>
  </NodeTemplate>
</template>
```

##### AddResearchButtonNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import ButtonNodeTemplate from '@/common/components/VueFlow/ButtonNodeTemplate/ButtonNodeTemplate.vue'
import { GLOBAL_NODE_HANDLES } from '@/common/components/VueFlow/global-nodes.types'
import { useNodes } from '@/common/composables/VueFlow/use-nodes/use-nodes'
import { RESEARCHS_NODE_TYPES } from '@/features/student/buildProject/views/BuildProjectView/sections/BuildProjectSection/components/MindMap/features/researchs/researchs-nodes.types'
import { MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { useI18n } from 'vue-i18n'

const { id } = defineProps<NodeProps>()

const { t } = useI18n()
const { addNode } = useNodes()

const researchNodesCount = ref(0)

function addResearchNode () {
  addNode({
    id: `research-${crypto.randomUUID()}`,
    type: RESEARCHS_NODE_TYPES.RESEARCH,
    position: { x: 100, y: researchNodesCount.value * 140 },
    data: {
      title: `Fiche ${researchNodesCount.value + 1}`,
      description: 'Description de la recherche...',
      left: true,
    },
    parentId: id,
    parentHandle: GLOBAL_NODE_HANDLES.RIGHT,
  })
  researchNodesCount.value += 1
}
</script>

<template>
  <ButtonNodeTemplate
    v-bind="$props"
    :label="t('student.buildProject.mindMap.researchs.addResearchButton.label')"
    :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
    @click="addResearchNode"
  />
</template>
```

##### TextInputNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import NodeTemplate from '@/common/components/VueFlow/NodeTemplate/NodeTemplate.vue'
import { AvInput } from '@avenirs-esr/avenirs-dsav'

defineProps<NodeProps>()
</script>

<template>
  <NodeTemplate
    v-bind="$props"
    collapsible
  >
    <template #title>
      <div class="av-row av-row--middle av-row--between av-flex-row-sm">
        <AvInput v-model="data.title" />
      </div>
    </template>

    <AvInput
      v-model="data.content"
      is-textarea
      row="4"
      @mousedown.stop
      @touchstart.stop
      @wheel.stop
    />
  </NodeTemplate>
</template>
```

##### LinkInputNode.vue [⇧](#table-des-matières)

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import NodeTemplate from '@/common/components/VueFlow/NodeTemplate/NodeTemplate.vue'
import { AvInput } from '@avenirs-esr/avenirs-dsav'

defineProps<NodeProps>()
</script>

<template>
  <NodeTemplate
    v-bind="$props"
    collapsible
  >
    <template #title>
      <a
        :href="data.link"
        target="_blank"
      >{{ data.link }}</a>
    </template>

    <AvInput v-model="data.link" />
  </NodeTemplate>
</template>
```