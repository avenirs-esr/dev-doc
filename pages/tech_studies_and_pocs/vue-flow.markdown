---
layout: page
title: Documentation VueFlow
permalink: /vue-flow/
---

_Last updated: 2025-12-11_

## Table des matières

- [Ressources](#ressources-)
- [Installation](#installation-)
  - [VueFlow](#vueflow-)
  - [html-to-image](#html-to-image-)
- [Premiers pas](#premiers-pas-)
  - [Initialisation de VueFlow](#initialisation-de-vueflow-)
  - [Ajout de noeuds](#ajout-de-noeuds-)
  - [Suppression de noeuds](#suppression-de-noeuds-)
  - [Connecter manuellement des noeuds](#connecter-manuellement-des-noeuds-)
- [Pour aller plus loin](#pour-aller-plus-loin-)
  - [Noeuds personnalisés](#noeuds-personnalisés-)
  - [Méthode dans les data d'un noeud personnalisé](#méthode-dans-les-data-dun-noeud-personnalisé-)
  - [Capture d'écran](#capture-décran-)
  - [`localStorage` et gestion de l'état](#localstorage-et-gestion-de-létat-)
    - [Sauvegarde de l'état courant](#sauvegarde-de-létat-courant-)
    - [Chargement du dernier état sauvegardé](#chargement-du-dernier-état-sauvegardé-)
    - [Bug connu - Méthodes de `data` non définies au chargement](#-bug-connu---méthodes-de-data-non-définies-au-chargement-)
- [Fonctions utiles](#fonctions-utiles-)
  - [getEdgeId](#getedgeid-)
- [Composables utiles](#composables-utiles-)
  - [useNodes](#usenodes-)
  - [useEdges](#useedges-)
    - [useEdges - version simplifiée](#useedges---version-simplifiée-)
  - [useHistory](#usehistory-)
  - [useFlowScreenshot](#useflowscreenshot-)
  - [useFlowState](#useflowstate-)
- [Composants utiles](#composants-utiles-)
  - [NodeDropdown](#nodedropdown-)
  - [Handles](#handles-)
    - [Handles - version simplifiée](#handles---version-simplifiée-)
  - [UpdateHandleSelector](#updatehandleselector-)
    - [UpdateHandleSelector - version simplifiée](#updatehandleselector---version-simplifiée-)
  - [UpdateHandlesModal](#updatehandlesmodal-)
    - [UpdateHandlesModal - version simplifiée](#updatehandlesmodal---version-simplifiée-)
  - [NodeTemplate](#nodetemplate-)
  - [ButtonNodeTemplate](#butonnodetemplate-)
  - [TitleDescriptionNodeTemplate](#titledescriptionnodetemplate-)


## Ressources [⇧](#table-des-matières)

- [vueflow.dev](https://vueflow.dev/guide/){:target="_blank"}
- [POC VueFlow](../vue-flow-poc){:target="_blank"}


## Installation [⇧](#table-des-matières)

### VueFlow [⇧](#table-des-matières)

Afin d'installer VueFlow dans un projet front, exécuter la commande suivante :
```
npm install @vue-flow/core
```

Pour que VueFlow s'affiche correctement, ajouter ceci dans le `main.ts` de votre projet :
```js
/* these are necessary styles for vue flow */
import '@vue-flow/core/dist/style.css'
/* this contains the default theme, these are optional styles */
import '@vue-flow/core/dist/theme-default.css'
```

### html-to-image [⇧](#table-des-matières)

Afin de pouvoir utiliser la fonctionnalité d'export en image, il faut installer `html-to-image` en version `1.11.11` :
```
npm install html-to-image@1.11.11
```

## Premiers pas [⇧](#table-des-matières)

### Initialisation de VueFlow [⇧](#table-des-matières)

Afin d'intégrer une instance de VueFlow dans votre projet, il faut instancier le composant `VueFlow` dans une `div` avec une hauteur et une largeur prédéfinie et ne pas oublier de lui passer des ref `nodes` et `edges`. Par exemple :

```js
// PocVueFlow.vue

<script lang="ts" setup>
import { type Edge, type Node, VueFlow } from '@vue-flow/core'

// === Initial nodes ===
const nodes = ref<Node[]>([
  {
    id: 'first-node',
    position: { x: 50, y: 50 },
  },
  {
    id: 'second-node',
    position: { x: 150, y: 150 },
  }
])

// === Initial edges ===
const edges = ref<Edge[]>([
  {
    id: 'efirst-node->second-node',
    source: 'first-node',
    target: 'second-node',
  },
])
</script>

<template>
  <div class="vue-flow-container">
    <VueFlow
      :nodes="nodes"
      :edges="edges"
    />
  </div>
</template>

<style lang="scss" scoped>
.vue-flow-container {
  border: 1px solid black;
  height: 80vh;
  width: 100%;
}

:deep(.vue-flow__handle) {
  &.source {
    background: var(--dark-background-primary1);
    border-color: var(--other-background-base);
  }

  &.target {
    background: var(--other-background-base);
    border-color: var(--dark-background-primary1);
  }
}
</style>
```

N.B.: La partie `:deep(.vue-flow__handle)` est un choix personnel permettant d'afficher plus clairement la distinction entre un point d'ancrage `source` et un point d'ancrage `target`.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_init.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : initialisation</figcaption>
</figure>
<br />

Une autre solution, qui sera celle à privilégier, consiste à récupérer les ref depuis `useVueFlow` afin de pouvoir `set` ou `add` des `nodes` et `edges` depuis n'importe où. Par exemple :

```js
// use-vue-flow-utils.ts

import { type Edge, type Node, useVueFlow } from '@vue-flow/core'

export function useVueFlowUtils () {
  const { setEdges, setNodes } = useVueFlow()

  // === Initial nodes ===
  const initialNodes = ref<Node[]>([
    {
      id: 'first-node',
      position: { x: 50, y: 50 },
    },
    {
      id: 'second-node',
      position: { x: 150, y: 150 },
    }
  ])

  // === Initial edges ===
  const initialEdges = ref<Edge[]>([
    {
      id: 'efirst-node->second-node',
      source: 'first-node',
      target: 'second-node',
    },
  ])

  // === VueFlow instance initialisation ===
  function initVueFlow () {
    setNodes(initialNodes.value)
    setEdges(initialEdges.value)
  }

  return {
    initVueFlow
  }
}
```

Une fois l'utilitaire d'initialisation créé, on peut l'appeler dans notre composant Vue :

```js
// PocVueFlow.vue

<script lang="ts" setup>
import { useVueFlowUtils } from '@/composables/use-vue-flow-utils'
import { useVueFlow, VueFlow } from '@vue-flow/core'

const { edges, nodes } = useVueFlow()
const { initVueFlow } = useVueFlowUtils()

initVueFlow()
</script>

<template>
  <div class="vue-flow-container">
    <VueFlow
      :nodes="nodes"
      :edges="edges"
    />
  </div>
</template>

<style lang="scss" scoped>
.vue-flow-container {
  border: 1px solid black;
  height: 80vh;
  width: 100%;
}

:deep(.vue-flow__handle) {
  &.source {
    background: var(--dark-background-primary1);
    border-color: var(--other-background-base);
  }

  &.target {
    background: var(--other-background-base);
    border-color: var(--dark-background-primary1);
  }
}
</style>
```

Le rendu ici est le même que précédemment.

### Ajout de noeuds [⇧](#table-des-matières)

Afin d'ajouter des noeuds à notre flow, le plus simple reste de créer une fonction utilitaire dans le composable `useVueFlowUtils` et de l'appeler lorsque cela est nécessaire.

```js
// use-vue-flow-utils.ts

import { type Edge, type Node, useVueFlow } from '@vue-flow/core'

export function useVueFlowUtils() {
  const { addNodes /* ... */ } = useVueFlow()

  // ...

  function addNode () {
    addNodes({
      id: `node-${crypto.randomUUID()}`,
      position: {
        x: Math.random() * 1200 + 50,
        y: Math.random() * 600 + 50,
      },
    })
  }

  return {
    // ...
    addNode
  }
}
```

Il est également possible d'ajouter automatiquement un lien lors de la création de noeuds :

```js
// use-vue-flow-utils.ts

import { type Node, useVueFlow } from '@vue-flow/core'

export function useVueFlowUtils() {
  const { nodes, addEdges, addNodes /* ... */ } = useVueFlow()

  // ...

  function addNode () {
    const newNode = {
      id: `node-${crypto.randomUUID()}`,
      position: {
        x: Math.random() * 1200 + 50,
        y: Math.random() * 600 + 50,
      },
    }

    addNodes(newNode)

    // Create a new edge from last node (the one before newNode already added) if it exists to the newNode
    if (nodes.value.length > 1) {
      addEdges([{
        source: nodes.value[nodes.value.length - 2].id,
        target: newNode.id,
      }])
    }
  }

  return {
    // ...
    addNode
  }
}
```

Ensuite, vous pouvez instancier un bouton qui appellera cette fonction pour créer des noeuds :

```js
// PocVueFlow.vue

<script lang="ts" setup>
import { useVueFlowUtils } from '@/composables/use-vue-flow-utils'
import { AvButton, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { useVueFlow, VueFlow } from '@vue-flow/core'

const { edges, nodes } = useVueFlow()
const { initVueFlow, addNode } = useVueFlowUtils()

initVueFlow()
</script>

<template>
  <div class="av-row av-flex-row-sm">
    <AvButton
      label="Ajouter un noeud"
      @click="addNode"
    />
  </div>
  <div class="vue-flow-container">
    <VueFlow
      :nodes="nodes"
      :edges="edges"
    />
  </div>
</template>

<style lang="scss" scoped>
.vue-flow-container {
  border: 1px solid black;
  height: 80vh;
  width: 100%;
}
</style>
```

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_add_nodes.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : ajout de noeuds</figcaption>
</figure>
<br />

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_add_nodes_with_edges.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : ajout de noeuds avec liens</figcaption>
</figure>
<br />

### Suppression de noeuds [⇧](#table-des-matières)

Afin de supprimer des noeuds de notre flow, le plus simple reste de créer une fonction utilitaire dans le composable `useVueFlowUtils` et de l'appeler lorsque cela est nécessaire.
Pour cette partie, afin de ne pas trop rentrer dans les fonctionnalités plus complexes, la méthode permettra de supprimer le dernier noeud de la liste.

```js
// use-vue-flow-utils.ts

import { type Edge, type Node, useVueFlow } from '@vue-flow/core'

export function useVueFlowUtils() {
  const { nodes, removeEdges, removeNodes /* ... */ } = useVueFlow()

  // ...

  function removeLastNode () {
    if (nodes.value.length === 0) {
        return
    }

    const lastNode = nodes.value[nodes.value.length - 1]
    removeNodes([lastNode.id])
    removeEdges(edges.value.filter(edge => edge.source === lastNode.id || edge.target === lastNode.id))
  }

  return {
    // ...
    removeLastNode
  }
}
```

Ensuite, vous pouvez instancier un bouton qui appellera cette fonction pour supprimer le noeud :

```js
// PocVueFlow.vue

<script lang="ts" setup>
import { useVueFlowUtils } from '@/composables/use-vue-flow-utils'
import { AvButton, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { useVueFlow, VueFlow } from '@vue-flow/core'

const { nodes, edges } = useVueFlow()
const { initVueFlow, addNode, removeLastNode } = useVueFlowUtils()

initVueFlow()
</script>

<template>
  <div class="av-flex-col-xs">
    <div class="av-row av-flex-row-sm">
      <AvButton
        label="Ajouter un noeud"
        :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
        @click="addNode"
      />
      <AvButton
        label="Supprimer le dernier noeud"
        :icon="MDI_ICONS.TRASH_CAN_OUTLINE"
        @click="removeLastNode"
      />
    </div>
    <div class="vue-flow-container">
      <VueFlow
        :nodes="nodes"
        :edges="edges"
      />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.vue-flow-container {
  border: 1px solid black;
  height: 80vh;
  width: 100%;
}

:deep(.vue-flow__handle) {
  &.source {
    background: var(--dark-background-primary1);
    border-color: var(--other-background-base);
  }

  &.target {
    background: var(--other-background-base);
    border-color: var(--dark-background-primary1);
  }
}
</style>
```

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_remove_nodes.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : suppression de noeuds</figcaption>
</figure>
<br />

### Connecter manuellement des noeuds [⇧](#table-des-matières)

Nous avons vu qu'il était possible de créer des connexion entre noeuds de manière automatique à l'ajout de noeuds. Jjusqu'à présent, il ne se passait rien lorsque l'on reliait deux noeuds manuellement ensemble. Cependant, il serait intéressant de pouvoir créer cette connexion à la main. Et VueFlow autorise ce fonctionnement. Pour cela, il faut définir une fonction de connexion et la passer au composant `VueFlow` via l'event `@connect`. Pour ce faire, je définis une méthode `onConnect` dans mon composable `useVueFlowUtils`.

```js
// use-vue-flow-utils.ts

import { type Connection, type Edge, type Node, useVueFlow } from '@vue-flow/core'

export function useVueFlowUtils() {
  const { edges, addEdges /* ... */ } = useVueFlow()

  // ...

  function onConnect (connection: Connection) {
    const newEdge: Edge = {
      ...connection,
      type: 'smoothstep',
      id: `e${connection.source}:${connection.sourceHandle}->${connection.target}:${connection.targetHandle}`,
    }

    if (edges.value.find(edge => edge.id === newEdge.id)) {
      return
    }

    addEdges([newEdge])
  }

  return {
    // ...
    onConnect
  }
}
```

N.B.: Ici, vous voyez apparaître `type: 'smoothstep'` dans la création des noeuds. C'est une propriété que je vais également dans chacun des codes précédents dans lesquels des `edge` étaient créés. Cela permet d'avoir des liens en lignes droites et non plus en lignes courbes. La mise à jour à faire est uniquement dans `use-vue-low-utils.ts` :

```js
// use-vue-flow-utils.ts

// ...

export function useVueFlowUtils() {
  // ...

  // === Initial edges ===
  const initialEdges = ref<Edge[]>([
    {
      id: 'efirst-node->second-node',
      source: 'first-node',
      target: 'second-node',
      type: 'smoothstep', // <-- ici
    },
  ])

  // ...

  function addNode () {
    // ...

    if (nodes.value.length > 1) {
      addEdges([{
        source: nodes.value[nodes.value.length - 2].id,
        target: newNode.id,
        type: 'smoothstep', // <-- et ici
      }])
    }
  }

  // ...
}
```

Il ne reste plus qu'à passer la méthode `onConnect` à l'event `@connect` du composant `VueFlow`.

```js
// PocVueFlow.vue

<script lang="ts" setup>
import { useVueFlowUtils } from '@/composables/use-vue-flow-utils'
import { AvButton, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { useVueFlow, VueFlow } from '@vue-flow/core'

const { nodes, edges } = useVueFlow()
const { initVueFlow, addNode, removeLastNode, onConnect } = useVueFlowUtils()

initVueFlow()
</script>

<template>
  <div class="av-flex-col-xs">
    <div class="av-row av-flex-row-sm">
      <AvButton
        label="Ajouter un noeud"
        :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
        @click="addNode"
      />
      <AvButton
        label="Supprimer le dernier noeud"
        :icon="MDI_ICONS.TRASH_CAN_OUTLINE"
        @click="removeLastNode"
      />
    </div>
    <div class="vue-flow-container">
      <VueFlow
        :nodes="nodes"
        :edges="edges"
        @connect="onConnect"
      />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.vue-flow-container {
  border: 1px solid black;
  height: 80vh;
  width: 100%;
}

:deep(.vue-flow__handle) {
  &.source {
    background: var(--dark-background-primary1);
    border-color: var(--other-background-base);
  }

  &.target {
    background: var(--other-background-base);
    border-color: var(--dark-background-primary1);
  }
}
</style>
```

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_manual_connection.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : ajout manuel de liens (type par défaut)</figcaption>
</figure>
<br />

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_manual_connection_smoothstep.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : ajout manuel de liens (type `smoothstep`)</figcaption>
</figure>
<br />


## Pour aller plus loin [⇧](#table-des-matières)

### Noeuds personnalisés [⇧](#table-des-matières)

Jusqu'ici, nous avons travaillé avec les noeuds par défaut de VueFlow. Cependant, il est possible de créer des noeuds personnalisés pour avoir des rendus spécifiques à des comportements attendus.

Dans un premier temps, il faudra créer un fichier Vue correspondant à ce noeud personnalisé. Je donne ici l'exemple de `LinkInputNode` qui permet d'afficher dans le noeud une `AvCard` collapsible avec dans le slot `title` une balise `a` dont le contenu dépend du texte renseigné dans l'`AvInput` présent dans le slot `default`.

Ici plusieurs subtilités :
- Il faut absolument une `div` en élément racine d'un noeud personnalisé pour que VueFlow envoie les props correctement.
- Chaque noeud custom contiendra a minima un `Handle`. Ce composant représente les points d'ancrage (`source` ou `target`) des liens des noeuds.
- Une propriété `data` découle de `NodeProps`. Cette propriété peut contenir absolument tout ce que vous désirez, des propriétés custom (comme mes `link` et `targetPosition`) ou des méthodes (j'y reviendrai plus loin).

```js
// LinkInputNode.vue

<script setup lang="ts">
import { AvCard, AvInput } from '@avenirs-esr/avenirs-dsav'
import { Handle, type NodeProps } from '@vue-flow/core'

defineProps<NodeProps>()
</script>

<template>
  <div>
    <AvCard collapsible>
      <template #title>
        <div class="av-row av-flex-row-sm av-row--between av-row--middle">
          <a
            :href="data.link"
            target="_blank"
          >{{ data.link }}</a>
        </div>
      </template>

      <AvInput v-model="data.link" />
    </AvCard>

    <Handle
      id="target"
      type="target"
      :position="data.targetPosition"
    />
  </div>
</template>
```

Afin d'avoir des méthodes utilitaires dédiées à ce type de noeud, je préconise de créer un composable dédié `useLinkInputFlow`. Pour l'exemple, je n'ajoute pour le moment qu'une fonction utilitaire permettant l'ajout du noeud.

Ici la création de noeud est quasiment identique à ce que nous avons vu précédemment. Voici les subtilités :
- Une propriété `type` est ajoutée et permettra d'indiquer à VueFlow que ces nouveaux noeuds seront de type `link-input`. J'explique juste après à quoi ça sert.
- Une propriété `data` qui, comme je l'ai dit plus haut, peut contenir tout ce que vous voulez, contient le `link` et la postion du point d'ancrage `target` des liens (`edge`).

```js
// use-link-input-flow.ts

import { type Node, Position, useVueFlow } from '@vue-flow/core'

interface UseLinkInputFlowReturn {
  addLinkInputNode: () => void
}

export function useLinkInputFlow (): UseLinkInputFlowReturn {
  const { addNodes } = useVueFlow()

  function addLinkInputNode () {
    const newNode: Node = {
      id: `link-input-${crypto.randomUUID()}`,
      type: 'link-input',
      position: {
        x: Math.random() * 1200 + 50,
        y: Math.random() * 600 + 50,
      },
      data: {
        link: '',
        targetPosition: Position.Left
      },
    }
    addNodes(newNode)
  }

  return {
    addLinkInputNode,
  }
}

```

Ensuite, pour intégrer ce nouveau type de noeud il faut expliquer à VueFlow que ce type de noeud existe. Pour ce faire, il faut ajouter un template `node-${nodeType}` (ici `node-link-input`) dans lequel on intégre le composant `LinkInputNode` avec les propriétés spécifiques au template et qui permettent d'accéder aux `NodeProps`.

```js
// PocVueFlow.vue

<script lang="ts" setup>
import LinkInputNode from '@/components/LinkInputNode/LinkInputNode.vue'
import { useLinkInputFlow } from '@/composables/use-link-input-flow'
import { useVueFlowUtils } from '@/composables/use-vue-flow-utils'
import { AvButton } from '@avenirs-esr/avenirs-dsav'
import { useVueFlow, VueFlow } from '@vue-flow/core'

const { edges, nodes } = useVueFlow()
const { initVueFlow, addNode, removeLastNode } = useVueFlowUtils()
const { addLinkInputNode } = useLinkInputFlow()

initVueFlow()
</script>

<template>
  <div class="av-flex-col-xs">
    <div class="av-row av-flex-row-sm">
      <AvButton
        label="Ajouter un noeud"
        :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
        @click="addNode"
      />
      <AvButton
        label="Ajouter un noeud de type link"
        :icon="MDI_ICONS.LINK"
        @click="addLinkInputNode"
      />
      <AvButton
        label="Supprimer le dernier noeud"
        :icon="MDI_ICONS.TRASH_CAN_OUTLINE"
        @click="removeLastNode"
      />
    </div>
    <div class="vue-flow-container">
      <VueFlow
        :nodes="nodes"
        :edges="edges"
      />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.vue-flow-container {
  border: 1px solid black;
  height: 80vh;
  width: 100%;
}

:deep(.vue-flow__handle) {
  &.source {
    background: var(--dark-background-primary1);
    border-color: var(--other-background-base);
  }

  &.target {
    background: var(--other-background-base);
    border-color: var(--dark-background-primary1);
  }
}
</style>
```

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_link_input_node.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : noeud personnalisé (<code>LinkInputNode</code>)</figcaption>
</figure>
<br />

### Méthode dans les data d'un noeud personnalisé [⇧](#table-des-matières)

Comme je l'ai expliqué, la propriété `data` d'un noeud personnalisé permet de passer plusieurs choses aux props de l'objet, des propriétés mais aussi des méthodes.

Supposons que j'aimerais avoir un bouton de suppression de mon noeud et passer la méthode de suppression par les `data`.

```js
// use-link-input-flow.ts

import { type Node, type Position, useVueFlow } from '@vue-flow/core'

interface UseLinkInputFlowReturn {
  addLinkInputNode: () => void
}

export function useLinkInputFlow (): UseLinkInputFlowReturn {
  const { addNodes, removeNodes, removeEdges } = useVueFlow()

  function removeNodeById (nodeId: string) {
    removeNodes([nodeId])
    removeEdges(edges.value.filter(edge => edge.source === nodeId || edge.target === nodeId))
  }

  function addLinkInputNode () {
    const newNodeId = `link-input-${crypto.randomUUID()}`

    const newNode: Node = {
      id: newNodeId,
      type: 'link-input',
      position: {
        x: 100,
        y: 100,
      },
      data: {
        link: '',
        targetPosition: Position.Left,
        removeNode: () => removeNodeById(newNodeId)
      },
    }
    addNodes(newNode)
  }

  return {
    addLinkInputNode,
  }
}

```

Ainsi, lorsque je créerai un noeud `LinkInputNode` avec `addLinkInputNode`, la méthode `removeNode` sera portée via `data` et je pourrai l'utiliser dans le composant `LinkInputNode` de cette façon :

```js
// LinkInputNode.vue

<script setup lang="ts">
import { AvButton, AvCard, AvInput, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { Handle, type NodeProps } from '@vue-flow/core'

defineProps<NodeProps>()
</script>

<template>
  <div>
    <AvCard collapsible>
      <template #title>
        <div class="av-row av-flex-row-sm av-row--between av-row--middle">
          <a
            :href="data.link"
            target="_blank"
          >{{ data.link }}</a>

          <AvButton
            label="Supprimer"
            :icon="MDI_ICONS.TRASH_CAN_OUTLINE"
            icon-only
            small
            @click="data.onRemove"
          />
        </div>
      </template>

      <AvInput v-model="data.link" />
    </AvCard>

    <Handle
      id="target"
      type="target"
      :position="data.targetPosition"
    />
  </div>
</template>
```

N.B.: Ici, j'aurais pu définir la méthode directement dans `LinkInputNode` sans passer par `data`. L'intérêt est de présenter un cas simple de passage de méthodes dans `data`.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_remove_link_input_node.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : Suppression d'un noeud via <code>data</code></figcaption>
</figure>
<br />

### Capture d'écran [⇧](#table-des-matières)

Afin de réaliser des captures d'écran, il vous faudra créer un composable `useScreenshot` qui vous permettra d'accéder à des fonctions utilitaires de `html-to-image`. Le code suivant est [le code d'exemple de screenshot](https://vueflow.dev/examples/screenshot.html) proposé par VueFlow (sur la page, chercher `useSreenshot.ts` dans l'arborescence du projet).

```js
// use-screenshot.ts

import type { Options as HTMLToImageOptions } from 'html-to-image/es/types'
import type { Ref } from 'vue'
import { toJpeg as ElToJpg, toPng as ElToPng } from 'html-to-image'

export type ImageType = 'jpeg' | 'png'

export interface UseScreenshotOptions extends HTMLToImageOptions {
  type?: ImageType
  fileName?: string
  shouldDownload?: boolean
  fetchRequestInit?: RequestInit
}

export type CaptureScreenshot = (
  el: HTMLElement,
  options?: UseScreenshotOptions
) => Promise<string>

export type Download = (fileName: string) => void

export interface UseScreenshot {
  // returns the data url of the screenshot
  capture: CaptureScreenshot
  download: Download
  dataUrl: Ref<string>
  error: Ref
}

export function useScreenshot (): UseScreenshot {
  const dataUrl = ref<string>('')
  const imgType = ref<ImageType>('png')
  const error = ref()

  async function capture (el: HTMLElement, options: UseScreenshotOptions = {}) {
    let data

    const fileName = options.fileName ?? `vue-flow-screenshot-${Date.now()}`

    switch (options.type) {
      case 'jpeg':
        data = await toJpeg(el, options)
        break
      case 'png':
        data = await toPng(el, options)
        break
      default:
        data = await toPng(el, options)
        break
    }

    // immediately download the image if shouldDownload is true
    if (options.shouldDownload && fileName !== '') {
      download(fileName)
    }

    return data
  }

  function toJpeg (
    el: HTMLElement,
    options: HTMLToImageOptions = { quality: 0.95 }
  ) {
    error.value = null

    return ElToJpg(el, options)
      .then((data) => {
        dataUrl.value = data
        imgType.value = 'jpeg'
        return data
      })
      .catch((error) => {
        error.value = error
        throw new Error(error)
      })
  }

  function toPng (
    el: HTMLElement,
    options: HTMLToImageOptions = { quality: 0.95 }
  ) {
    error.value = null

    return ElToPng(el, options)
      .then((data) => {
        dataUrl.value = data
        imgType.value = 'png'
        return data
      })
      .catch((error) => {
        error.value = error
        throw new Error(error)
      })
  }

  function download (fileName: string) {
    const link = document.createElement('a')
    link.download = `${fileName}.${imgType.value}`
    link.href = dataUrl.value
    link.click()
  }

  return {
    capture,
    download,
    dataUrl,
    error,
  }
}
```

Vous pouvez enuite appeler la méthode `capture` retournée par `useScreenshot` dans un composable personnalisé pour créer une méthode `doScreenshot`. Cette méthode sera à associer à un bouton permettant de faire la capture d'écran. Étant donné qu'il s'agira ici d'un utilitaire VueFlow, j'ai fait le choix de déclarer ceci dans `useVueFlowUtils`. Le code de la méthode `doScreenshot` provient également [du code d'exemple de screenshot](https://vueflow.dev/examples/screenshot.html) proposé par VueFlow.

```js
// use-vue-flow-utils.ts

import { useScreenshot } from '@/composables/use-screenshot'
import { useVueFlow /* ... */ } from '@vue-flow/core'

export function useVueFlowUtils () {
  const { vueFlowRef /* ... */ } = useVueFlow()

  // ...

  function doScreenshot (fileName?: string) {
    if (!vueFlowRef.value) {
      console.warn('VueFlow element not found')
      return
    }

    capture(vueFlowRef.value, { shouldDownload: true, fileName: `${fileName ?? 'vue-flow'}-screenshot-${Date.now()}` })
  }

  return {
    // ...
    doScreenshot
  }
}
```

N.B.: Ici, `shouldDownload` est passé à `true` dans `capture`. Cela permettra de télécharger directement l'image à chaque appel à `doScreenshot`.

Il ne reste plus qu'à appeler cette méthode dans un bouton pour pouvoir faire une capture d'écran de VueFlow.

```js
// PocVueFlow.vue

<script lang="ts" setup>
import LinkInputNode from '@/components/LinkInputNode.vue'
import { useLinkInputFlow } from '@/composables/use-link-input-flow'
import { useVueFlowUtils } from '@/composables/use-vue-flow-utils'
import { AvButton, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { useVueFlow, VueFlow } from '@vue-flow/core'

const { nodes, edges } = useVueFlow()
const { initVueFlow, addNode, removeLastNode, onConnect, doScreenshot } = useVueFlowUtils()
const { addLinkInputNode } = useLinkInputFlow()

initVueFlow()
</script>

<template>
  <div class="av-flex-col-xs">
    <div class="av-row av-flex-row-sm">
      <AvButton
        label="Ajouter un noeud"
        :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
        @click="addNode"
      />
      <AvButton
        label="Ajouter un noeud de type link"
        :icon="MDI_ICONS.LINK"
        @click="addLinkInputNode"
      />
      <AvButton
        label="Supprimer le dernier noeud"
        :icon="MDI_ICONS.TRASH_CAN_OUTLINE"
        @click="removeLastNode"
      />
      <AvButton
        label="Capture d'écran"
        :icon="MDI_ICONS.FILE_IMAGE_OUTLINE"
        @click="() => doScreenshot()"
      />
    </div>
    <div class="vue-flow-container">
      <VueFlow
        :nodes="nodes"
        :edges="edges"
        @connect="onConnect"
      >
        <template #node-link-input="linkInputNodeProps">
          <LinkInputNode v-bind="linkInputNodeProps" />
        </template>
      </VueFlow>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.vue-flow-container {
  border: 1px solid black;
  height: 80vh;
  width: 100%;
}

:deep(.vue-flow__handle) {
  &.source {
    background: var(--dark-background-primary1);
    border-color: var(--other-background-base);
  }

  &.target {
    background: var(--other-background-base);
    border-color: var(--dark-background-primary1);
  }
}
</style>
```

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_screenshot.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : Capture d'écran</figcaption>
</figure>
<br />

### `localStorage` et gestion de l'état [⇧](#table-des-matières)

Avoir une capture d'écran pour sauvegarder son VueFlow c'est bien. Mais avoir une vraie sauvegarde, ce serait mieux. Afin de gérer la sauvegarde de l'état de VueFlow et le chargement d'un état sauvegardé, nous allons passer par le `localStorage`. Nous y sauvegarderons l'ensemble des noeuds et des liens. Pour ce faire, nous allons créer un nouveau composable dédié à la gestion de l'état que nous appellerons `useVueFlowState`.

#### Sauvegarde de l'état courant [⇧](#table-des-matières)

Voyons dans un premier temps la sauvegarde. La méthode est assez triviale, il suffit de set des items nous intéressant dans le `localStorage`. Pour ce faire, on appelle la méthode `setItem` de `localStorage` avec un identifiant à donner à l'item (ici `vue-flow-nodes` et `vue-flow-edges`) et la valeur de l'item (ici le JSON stringifié des noeuds et des liens récupérés via `useVueFlow`).

```js
// use-vue-flow-state.ts

import { useToasterStore } from '@/store'
import { useVueFlow } from '@vue-flow/core'

export function useVueFlowState () {
  const { nodes, edges } = useVueFlow()
  const { addSuccessMessage } = useToasterStore()

  function saveCurrentState () {
    localStorage.setItem(`vue-flow-nodes`, JSON.stringify(nodes.value))
    localStorage.setItem(`vue-flow-edges`, JSON.stringify(edges.value))
    addSuccessMessage({ description: 'État du VueFlow sauvegardé avec succès.', timeout: 2000 })
  }

  return {
    saveCurrentState,
  }
}

```

N.B.: Ici, j'ai fait le choix d'ajouter un toaster indiquant que les éléments ont été sauvegardé. C'est toujours appréciable pour l'utilisateur de savoir que son action a fait quelque chose.

Il ne reste plus qu'à appeler cette méthode dans un bouton :

```js
// PocVueFlow.ts

<script lang="ts" setup>
import LinkInputNode from '@/components/LinkInputNode.vue'
import { useLinkInputFlow } from '@/composables/use-link-input-flow'
import { useVueFlowState } from '@/composables/use-vue-flow-state'
import { useVueFlowUtils } from '@/composables/use-vue-flow-utils'
import { AvButton, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { useVueFlow, VueFlow } from '@vue-flow/core'

const { nodes, edges } = useVueFlow()
const { initVueFlow, addNode, removeLastNode, onConnect, doScreenshot } = useVueFlowUtils()
const { addLinkInputNode } = useLinkInputFlow()
const { saveCurrentState } = useVueFlowState()

initVueFlow()
</script>

<template>
  <div class="av-flex-col-xs">
    <div class="av-row av-flex-row-sm">
      <AvButton
        label="Ajouter un noeud"
        :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
        @click="addNode"
      />
      <AvButton
        label="Ajouter un noeud de type link"
        :icon="MDI_ICONS.LINK"
        @click="addLinkInputNode"
      />
      <AvButton
        label="Supprimer le dernier noeud"
        :icon="MDI_ICONS.TRASH_CAN_OUTLINE"
        @click="removeLastNode"
      />
      <AvButton
        label="Capture d'écran"
        :icon="MDI_ICONS.FILE_IMAGE_OUTLINE"
        @click="() => doScreenshot()"
      />
      <AvButton
        label="Sauvegarder l'état actuel"
        :icon="MDI_ICONS.CONTENT_SAVE_OUTLINE"
        @click="saveCurrentState"
      />
    </div>
    <div class="vue-flow-container">
      <VueFlow
        :nodes="nodes"
        :edges="edges"
        @connect="onConnect"
      >
        <template #node-link-input="linkInputNodeProps">
          <LinkInputNode v-bind="linkInputNodeProps" />
        </template>
      </VueFlow>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.vue-flow-container {
  border: 1px solid black;
  height: 80vh;
  width: 100%;
}

:deep(.vue-flow__handle) {
  &.source {
    background: var(--dark-background-primary1);
    border-color: var(--other-background-base);
  }

  &.target {
    background: var(--other-background-base);
    border-color: var(--dark-background-primary1);
  }
}
</style>
```

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_save_state.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : Sauvegarde de l'état dans le <code>localStorage</code></figcaption>
</figure>
<br />

#### Chargement du dernier état sauvegardé [⇧](#table-des-matières)

Maintenant que nous sommes capables de sauvegarder l'état courant, il faudrait pouvoir le récupérer lorsque nous quittons puis revenons sur la page. Nous allons donc créer une méthode `restoreSavedState` dans le composable `useVueFlowState`. Nous récupérons les JSON sauvegardés dans le `localStorage` avec l'identifiant que nous avons choisi dans la méthode `saveCurrentStage` et faisons un parsing de ces JSON pour set les `nodes` et les `edges`.

```js
// use-vue-flow-state.ts

import { useToasterStore } from '@/store'
import { useVueFlow } from '@vue-flow/core'

export function useVueFlowState () {
  const { setNodes, setEdges } = useVueFlow()
  const { addMessage /* ... */ } = useToasterStore()

  // ...

  function restoreSavedState () {
    const savedNodes = localStorage.getItem('vue-flow-nodes')
    const savedEdges = localStorage.getItem('vue-flow-edges')

    if (!savedNodes?.length || !savedEdges?.length) {
      addMessage({ description: 'Aucun état sauvegardé trouvé pour ce VueFlow.', type: 'warning', timeout: 2000 })
      return
    }

    setNodes(JSON.parse(savedNodes))
    setEdges(JSON.parse(savedEdges))
  }

  return {
    // ...
    restoreSavedState
  }
}
```

N.B.: Ici, j'ai fait le choix d'ajouter un toaster indiquant à l'utilisateur qu'aucun état n'est sauvegardé si tel est le cas.

Il ne reste ensuite plus qu'à ajouter le bouton correspondant.

```js
// PocVueFlow.ts

<script lang="ts" setup>
import LinkInputNode from '@/components/LinkInputNode.vue'
import { useLinkInputFlow } from '@/composables/use-link-input-flow'
import { useVueFlowState } from '@/composables/use-vue-flow-state'
import { useVueFlowUtils } from '@/composables/use-vue-flow-utils'
import { AvButton, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { useVueFlow, VueFlow } from '@vue-flow/core'

const { nodes, edges } = useVueFlow()
const { initVueFlow, addNode, removeLastNode, onConnect, doScreenshot } = useVueFlowUtils()
const { addLinkInputNode } = useLinkInputFlow()
const { saveCurrentState, restoreSavedState } = useVueFlowState()

initVueFlow()
</script>

<template>
  <div class="av-flex-col-xs">
    <div class="av-row av-flex-row-sm">
      <AvButton
        label="Ajouter un noeud"
        :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
        @click="addNode"
      />
      <AvButton
        label="Ajouter un noeud de type link"
        :icon="MDI_ICONS.LINK"
        @click="addLinkInputNode"
      />
      <AvButton
        label="Supprimer le dernier noeud"
        :icon="MDI_ICONS.TRASH_CAN_OUTLINE"
        @click="removeLastNode"
      />
      <AvButton
        label="Capture d'écran"
        :icon="MDI_ICONS.FILE_IMAGE_OUTLINE"
        @click="() => doScreenshot()"
      />
      <AvButton
        label="Sauvegarder l'état actuel"
        :icon="MDI_ICONS.CONTENT_SAVE_OUTLINE"
        @click="saveCurrentState"
      />
      <AvButton
        label="Charger l'état sauvegardé"
        :icon="MDI_ICONS.SWAP_HORIZONTAL"
        @click="restoreSavedState"
      />
    </div>
    <div class="vue-flow-container">
      <VueFlow
        :nodes="nodes"
        :edges="edges"
        @connect="onConnect"
      >
        <template #node-link-input="linkInputNodeProps">
          <LinkInputNode v-bind="linkInputNodeProps" />
        </template>
      </VueFlow>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.vue-flow-container {
  border: 1px solid black;
  height: 80vh;
  width: 100%;
}

:deep(.vue-flow__handle) {
  &.source {
    background: var(--dark-background-primary1);
    border-color: var(--other-background-base);
  }

  &.target {
    background: var(--other-background-base);
    border-color: var(--dark-background-primary1);
  }
}
</style>
```

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_restore_state_without_save.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : Chargement de l'état sans état sauvegardé</figcaption>
</figure>
<br />

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_restore_state.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : Chargement de l'état sauvegardé</figcaption>
</figure>
<br />

#### ⚠ Bug connu - Méthodes de `data` non définies au chargement [⇧](#table-des-matières)

Attention. Nous avons vu que lors de la sauvegarde, les noeuds sont sauvegardés en JSON. De ce fait, les méthodes ne sont pas sauvegardées. Ainsi, lorsque nous chargeons les noeuds, les méthodes présentes dans `data` ne sont pas récupérées. Ainsi, par exemple, la méthode de suppression ajoutée à `LinkInputNode` n'est plus cliquable comme le montre la vidéo ci-après.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_restore_state_undefined_method.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : Méthode <code>onRemove</code> de <code>data</code> non définie au chargement</figcaption>
</figure>
<br />

Pas de panique, tout n'est pas perdu. Nous allons implémenter une méthode de réhydratation des noeuds. Commençons par créer la méthode de réhydratation `rehydrateLinkInputNode` qui va rajouter les méthodes disparues aux `data` des noeuds qui seront chargés.

```js
// use-link-input-flow.ts

import { type Node, Position, useVueFlow } from '@vue-flow/core'

interface UseLinkInputFlowReturn {
  addLinkInputNode: () => void
  rehydrateLinkInputNode: (node: Node) => Node
}

export function useLinkInputFlow (): UseLinkInputFlowReturn {
  const { addNodes, removeNodes, removeEdges, edges } = useVueFlow()

  function removeNodeById (nodeId: string) {
    removeNodes([nodeId])
    removeEdges(edges.value.filter(edge => edge.source === nodeId || edge.target === nodeId))
  }

  function addLinkInputNode () {
    const newNodeId = `link-input-${crypto.randomUUID()}`

    const newNode: Node = {
      id: newNodeId,
      type: 'link-input',
      position: {
        x: Math.random() * 1200 + 50,
        y: Math.random() * 600 + 50,
      },
      data: {
        link: '',
        targetPosition: Position.Left,
        onRemove: () => removeNodeById(newNodeId),
      },
    }
    addNodes(newNode)
  }

  function rehydrateLinkInputNode (node: Node) {
    if (node.type === 'link-input') {
      node.data = {
        ...node.data,
        onRemove: () => removeNodeById(node.id),
      }
    }

    return node
  }

  return {
    addLinkInputNode,
    rehydrateLinkInputNode
  }
}

```

Ensuite, il faut que `useVueFlowState` accepte en paramètre une méthode de réhydration générique.

```js
// use-vue-flow-state.ts

import { useToasterStore } from '@/store'
import { type Node, useVueFlow } from '@vue-flow/core'

interface UseVueFlowStateParams {
  rehydrateNodes: (nodes: Node[]) => Node[]
}

export function useVueFlowState ({ rehydrateNodes }: UseVueFlowStateParams) {
  const { setNodes, setEdges /* ... */ } = useVueFlow()

  // ...

  function restoreSavedState () {
    const savedNodes = localStorage.getItem('vue-flow-nodes')
    const savedEdges = localStorage.getItem('vue-flow-edges')

    // ...

    setNodes(rehydrateNodes(JSON.parse(savedNodes)))
    setEdges(JSON.parse(savedEdges))
  }

  return {
    saveCurrentState,
    restoreSavedState,
  }
}
```

Enfin, il faut créer cette méthode générie dans notre composant Vue `PocVueFlow`, appeler dans cette méthode `rehydrateLinkInputNode` et la passer en paramètre de `useVueFlowState`.

```js
// PocVueFlow.ts

<script lang="ts" setup>
import LinkInputNode from '@/components/LinkInputNode.vue'
import { useLinkInputFlow } from '@/composables/use-link-input-flow'
import { useVueFlowState } from '@/composables/use-vue-flow-state'
import { useVueFlowUtils } from '@/composables/use-vue-flow-utils'
import { AvButton, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { type Node, useVueFlow, VueFlow } from '@vue-flow/core'

const { nodes, edges } = useVueFlow()
const { initVueFlow, addNode, removeLastNode, onConnect, doScreenshot } = useVueFlowUtils()
const { addLinkInputNode, rehydrateLinkInputNode } = useLinkInputFlow()
const { saveCurrentState, restoreSavedState } = useVueFlowState({ rehydrateNodes })

function rehydrateNodes (dehydratedNodes: Node[]) {
  dehydratedNodes.forEach((node) => {
    rehydrateLinkInputNode(node)
  })

  return dehydratedNodes
}

initVueFlow()
</script>

<template>
  <div class="av-flex-col-xs">
    <div class="av-row av-flex-row-sm">
      <AvButton
        label="Ajouter un noeud"
        :icon="MDI_ICONS.PLUS_CIRCLE_OUTLINE"
        @click="addNode"
      />
      <AvButton
        label="Ajouter un noeud de type link"
        :icon="MDI_ICONS.LINK"
        @click="addLinkInputNode"
      />
      <AvButton
        label="Supprimer le dernier noeud"
        :icon="MDI_ICONS.TRASH_CAN_OUTLINE"
        @click="removeLastNode"
      />
      <AvButton
        label="Capture d'écran"
        :icon="MDI_ICONS.FILE_IMAGE_OUTLINE"
        @click="() => doScreenshot()"
      />
      <AvButton
        label="Sauvegarder l'état actuel"
        :icon="MDI_ICONS.CONTENT_SAVE_OUTLINE"
        @click="saveCurrentState"
      />
      <AvButton
        label="Charger l'état sauvegardé"
        :icon="MDI_ICONS.SWAP_HORIZONTAL"
        @click="restoreSavedState"
      />
    </div>
    <div class="vue-flow-container">
      <VueFlow
        :nodes="nodes"
        :edges="edges"
        @connect="onConnect"
      >
        <template #node-link-input="linkInputNodeProps">
          <LinkInputNode v-bind="linkInputNodeProps" />
        </template>
      </VueFlow>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.vue-flow-container {
  border: 1px solid black;
  height: 80vh;
  width: 100%;
}

:deep(.vue-flow__handle) {
  &.source {
    background: var(--dark-background-primary1);
    border-color: var(--other-background-base);
  }

  &.target {
    background: var(--other-background-base);
    border-color: var(--dark-background-primary1);
  }
}
</style>
```

Et maintenant, la méthode de suppression est à nouveau accessible au chargement.

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_restore_state_rehydrated_nodes.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : Chargement avec noeuds réhydratés - méthode <code>onRemove</code> à nouveau accessible</figcaption>
</figure>
<br />

N.B.: Même si une réhydratation des noeuds est possible, il faudra donc éviter au maximum de passer des méthodes dans les `data` des noeuds pour s'éviter des opérations supplémentaires.

## Fonctions utiles [⇧](#table-des-matières)

### getEdgeId [⇧](#table-des-matières)

Cette fonction permet de créer automatiquement un identifiant pour un lien entre deux noeuds en fonction d'un `Edge` passé en paramètres.

```js
// edges.ts

import type { Edge } from '@vue-flow/core'

export function getEdgeId (edge: Omit<Edge, 'id'>) {
  return `e${edge.source}:${edge.sourceHandle}->${edge.target}:${edge.targetHandle}`
}
```

## Composables utiles [⇧](#table-des-matières)

### useNodes [⇧](#table-des-matières)

Ce composable permet de gérer des noeuds dans un diagramme VueFlow. Il expose plusieurs fonctions utilitaires permettant de manipuler des noeuds et leurs relations.
Il expose notamment :
- `addNode`: fonction pour ajouter un noeud et un lien vers le parent s'il est renseigné
- `updateNodeId`: fonction pour mettre à jour l'ID d'un noeud (utile notamment lorsque l'on veut passer un ID reçu par query/mutation)
- `removeNode`: fonction pour supprimer un noeud grâce à son ID
- `removeNodeWithChildren`: fonction pour supprimer un noeud et tous ses enfants (récursivement) grâce à son ID
- `toggle`: fonction pour basculer l'état `collapsed` d'un noeud
- `findNodeByTitleAndDescription`: fonction pour trouver un noeud grâce aux propriétés `title` et `description` exposée dans sa `data`

```js
// use-nodes.ts

import { GLOBAL_NODE_HANDLES } from '@/common/components/VueFlow/global-nodes.types'
import { getEdgeId } from '@/common/utils/vue-flow/vue-flow'
import { type Edge, type Node, type NodeProps, Position, useVueFlow } from '@vue-flow/core'

interface AddNodeParams {
  id?: string
  type: string
  position?: { x: number, y: number }
  data?: NodeProps['data']
  parentId?: string
  parentHandle?: GLOBAL_NODE_HANDLES
  nodeHandle?: GLOBAL_NODE_HANDLES
  width?: NodeProps['dimensions']['width']
}
interface UseNodesReturn {
  /**
   * Function to add a new node and create an edge from a parent node to the new node.
   * The parameters for adding the new node and creating the edge are the following:
   * - `id`: Optional ID for the new node. If not provided, a random UUID will be generated.
   * - `type`: Type of the new node.
   * - `position`: Optional position for the new node. If not provided, a random position will be generated.
   * - `data`: Optional data for the new node.
   * - `parentId`: Optional ID of the parent node from which the edge will originate.
   * - `parentHandle`: Optional source handle on the parent node for the edge. Defaults to `GLOBAL_NODE_HANDLES.RIGHT`.
   * - `nodeHandle`: Optional target handle on the new node for the edge. Defaults to first true position in data or `GLOBAL_NODE_HANDLES.LEFT`.
   * - `width`: Optional width for the new node.
   */
  addNode: ({ id, type, position, data, parentId, parentHandle, nodeHandle, width }: AddNodeParams) => void

  /**
   * Function to update a node's ID and also update all edges and child nodes that reference the old ID.
   * @param nodeId Current ID of the node to be updated.
   * @param newId New ID to be assigned to the node.
   */
  updateNodeId: (nodeId: string, newId: string) => void

  /**
   * Function to remove a node by its ID.
   * Uses the `removeNodes` and `removeEdges` functions from Vue Flow to remove the node and its connected edges.
   * @param nodeId ID of the node to be removed.
   */
  removeNode: (nodeId: string) => void

  /**
   * Function to remove a node and all its child nodes recursively.
   * Uses the `removeNodes` and `removeEdges` functions from Vue Flow to remove the nodes and their connected edges.
   * @param nodeId ID of the node to be removed along with its children.
   */
  removeNodeWithChildren: (nodeId: string) => void

  /**
   * Function to toggle the collapsed state of a node.
   * Uses the `updateNode` function from Vue Flow to update the node `hidden` property and the `collapsed` property in `data`.
   * @param nodeId ID of the node to be toggled.
   */
  toggle: (nodeId: string) => void

  /**
   * Function to find a node by the title and description defined in its data.
   * @param title title of the node to be found.
   * @param description description of the node to be found.
   * @returns The found node or undefined if not found.
   */
  findNodeByTitleAndDescription: (title: string, description: string) => Node | undefined
}

/**
 * Composable for managing nodes in a Vue Flow diagram.
 * It provides several utility functions to manipulate nodes and their relationships.
 * @returns
 * - `removeNode`: Function to remove a node by its ID.
 * - `removeNodeWithChildren`: Function to remove a node and all its child nodes recursively.
 * - `toggle`: Function to toggle the collapsed state of a node.
 * - `findNodeByTitleAndDescription`: Function to find a node by the title and description defined in its data.
 */
export function useNodes (): UseNodesReturn {
  const { nodes, edges, addNodes, addEdges, findNode, removeNodes, removeEdges, updateNode, updateEdge } = useVueFlow()

  /**
   * Function to add a new node and create an edge from a parent node to the new node.
   * @param params Parameters for adding the new node and creating the edge.
   * @param "params.id": Optional ID for the new node. If not provided, a random UUID will be generated.
   * @param "params.type": Type of the new node.
   * @param "params.position": Optional position for the new node. If not provided, a random position will be generated.
   * @param "params.data": Optional data for the new node.
   * @param "params.parentId": Optional ID of the parent node from which the edge will originate.
   * @param "params.parentHandle": Optional source handle on the parent node for the edge. Defaults to `GLOBAL_NODE_HANDLES.RIGHT`.
   * @param "params.nodeHandle": Optional target handle on the new node for the edge. Defaults to first true position in data or `GLOBAL_NODE_HANDLES.LEFT`.
   * @param "params.width": Optional width for the new node.
   */
  function addNode ({ id, type, position, data, parentId, parentHandle, nodeHandle, width }: AddNodeParams) {
    const POSITION_ORDER: Position[] = [
      Position.Top,
      Position.Right,
      Position.Bottom,
      Position.Left,
    ]

    // === Create a new node ===
    const newNode: Node = {
      id: id ?? `${type}-${crypto.randomUUID()}`,
      type,
      parentNode: parentId,
      position: position ?? {
        x: Math.random() * 150,
        y: Math.random() * 150,
      },
      data,
      width,
    }
    addNodes(newNode)

    // === Create a new edge from the parent to the new node ===
    if (parentId) {
      const newEdgeWithoutId: Omit<Edge, 'id'> = {
        source: parentId,
        sourceHandle: parentHandle ?? GLOBAL_NODE_HANDLES.RIGHT,
        target: newNode.id,
        targetHandle: nodeHandle ?? POSITION_ORDER.find(pos => data[pos] === true) ?? GLOBAL_NODE_HANDLES.LEFT,
        type: 'smoothstep',
      }
      const newEdge: Edge = {
        ...newEdgeWithoutId,
        id: getEdgeId(newEdgeWithoutId),
      }

      addEdges([newEdge])
    }
  }

  /**
   * Function to update a node's ID and also update all edges and child nodes that reference the old ID.
   * @param nodeId Current ID of the node to be updated.
   * @param newId New ID to be assigned to the node.
   */
  function updateNodeId (nodeId: string, newId: string) {
    updateNode(nodeId, { id: newId })
    edges.value.forEach((edge) => {
      let updated = false
      const updatedEdge = { ...edge }

      if (edge.source === nodeId) {
        updatedEdge.source = newId
        updated = true
      }
      if (edge.target === nodeId) {
        updatedEdge.target = newId
        updated = true
      }

      if (updated) {
        updateEdge(edge, updatedEdge)
      }
    })
    nodes.value.forEach((node) => {
      if (node.parentNode === nodeId) {
        updateNode(node.id, { parentNode: newId })
      }
    })
  }

  /**
   * Function to remove a node by its ID.
   * Uses the `removeNodes` and `removeEdges` functions from Vue Flow to remove the node and its connected edges.
   * @param nodeId ID of the node to be removed.
   */
  function removeNode (nodeId: string) {
    removeNodes([nodeId])
    removeEdges(edges.value.filter(edge => edge.source === nodeId || edge.target === nodeId))
  }

  /**
   * Function to remove a node and all its child nodes recursively.
   * Uses the `removeNodes` and `removeEdges` functions from Vue Flow to remove the nodes and their connected edges.
   * @param nodeId ID of the node to be removed along with its children.
   */
  function removeNodeWithChildren (nodeId: string) {
    const node = findNode(nodeId)
    if (!node) {
      return
    }

    nodes.value.forEach((childNode) => {
      if (childNode.parentNode === nodeId) {
        removeNodeWithChildren(childNode.id)
      }
    })

    removeNode(nodeId)
  }

  /**
   * Function to find a node by the title and description defined in its data.
   * @param title title of the node to be found.
   * @param description description of the node to be found.
   * @returns The found node or undefined if not found.
   */
  function findNodeByTitleAndDescription (title: string, description: string) {
    return nodes.value.find(node => node.data.title === title && node.data.description === description)
  }

  /**
   * Function to hide all child nodes of a given parent node recursively.
   * Uses the `updateNode` function from Vue Flow to set the `hidden` property of child nodes to true.
   * @param parentId ID of the parent node.
   */
  function hideChildren (parentId: string) {
    nodes.value.forEach((node) => {
      if (node.parentNode === parentId) {
        updateNode(node.id, { hidden: true })
        hideChildren(node.id)
      }
    })
  }

  /**
   * Function to show all child nodes of a given parent node recursively.
   * Uses the `updateNode` function from Vue Flow to set the `hidden` property of child nodes to false.
   * @param parentId ID of the parent node.
   */
  function showChildren (parentId: string) {
    nodes.value.forEach((node) => {
      if (node.parentNode === parentId) {
        updateNode(node.id, { hidden: false })
        showChildren(node.id)
      }
    })
  }

  /**
   * Function to toggle the collapsed state of a node.
   * Uses the `updateNode` function from Vue Flow to update the node `hidden` property and the `collapsed` property in `data`.
   * @param nodeId ID of the node to be toggled.
   */
  function toggle (nodeId: string) {
    const node = findNode(nodeId)
    if (!node) {
      return
    }

    node.data.collapsed = !node.data.collapsed

    if (node.data.collapsed) {
      hideChildren(nodeId)
    }
    else {
      showChildren(nodeId)
    }

    updateNode(nodeId, { data: node.data })
  }

  return {
    addNode,
    updateNodeId,
    removeNode,
    removeNodeWithChildren,
    toggle,
    findNodeByTitleAndDescription
  }
}
```

### useEdges [⇧](#table-des-matières)

Pour le moment, ce composable ne sert qu'à définir la méthode `onConnect` à passer à `VueFlow`. Ici elle crée une connexion entre deux noeuds si la connexion est jugée valide.

```js
// use-edges.ts

import { getEdgeId } from '@/utils/edges'
import { type Connection, type Edge, useVueFlow } from '@vue-flow/core'

export function useEdges () {
  const { edges, addEdges } = useVueFlow()

  function isValidConnection (connection: Connection) {
    const from = connection.sourceHandle
    const to = connection.targetHandle

    if (!to) {
      console.warn('Invalid connection: target handle is missing')
      return false
    }

    if (!from) {
      console.warn('Invalid connection: source handle is missing')
      return false
    }

    if (from.includes('source') && to.includes('source')) {
      console.warn('Invalid connection: cannot connect two source handles')
      return false
    }

    if (from.includes('target') && to.includes('target')) {
      console.warn('Invalid connection: cannot connect two target handles')
      return false
    }

    return true
  }

  function onConnect (connection: Connection) {
    if (!isValidConnection(connection)) {
      return
    }

    const newEdge: Edge = {
      ...connection,
      type: 'smoothstep',
      id: getEdgeId(connection),
    }

    if (edges.value.find(edge => edge.id === newEdge.id)) {
      return
    }

    addEdges([newEdge])
  }

  return {
    onConnect,
  }
}
```

#### useEdges - version simplifiée [⇧](#table-des-matières)

Dans un cas où il n'y aurait pas de distinction entre `source` et `target` pour les points d'ancrage des liens, la simplification suivante peut être réalisée.

```js
// use-edges.ts

import { getEdgeId } from '@/utils/edges'
import { type Connection, type Edge, useVueFlow } from '@vue-flow/core'

interface UseEdgesReturn {
  /**
   * Handler for connecting two nodes in the flow. Uses the `addEdges` function from Vue Flow to add a new edge.
   * @param connection Connection object containing source and target information.
   */
  onConnect: (connection: Connection) => void
}

export function useEdges () {
  const { edges, addEdges } = useVueFlow()

  /**
   * Validates a connection between two nodes.
   * @param connection Connection object containing source and target information.
   * @returns Boolean indicating whether the connection is valid.
   */
  function isValidConnection (connection: Connection): UseEdgesReturn {
    const from = connection.sourceHandle
    const to = connection.targetHandle

    if (!to) {
      console.warn('Invalid connection: target handle is missing')
      return false
    }

    if (!from) {
      console.warn('Invalid connection: source handle is missing')
      return false
    }

    return true
  }

  /**
   * Handler for connecting two nodes in the flow. Uses the `addEdges` function from Vue Flow to add a new edge.
   * @param connection Connection object containing source and target information.
   */
  function onConnect (connection: Connection) {
    if (!isValidConnection(connection)) {
      return
    }

    const newEdge: Edge = {
      ...connection,
      type: 'smoothstep',
      id: getEdgeId(connection),
    }

    if (edges.value.find(edge => edge.id === newEdge.id)) {
      return
    }

    addEdges([newEdge])
  }

  return {
    onConnect,
  }
}
```

### useHistory [⇧](#table-des-matières)

Ce composable permet d'avoir accès aux utilitaires `undo` et `redo`. Pour cela, il faut absolument penser à appeler `saveSnapshot` avant toute action que l'on souhaite pouvoir intégrer à la pile.

```js
// use-history.ts

import { useVueFlow } from '@vue-flow/core'
import type { ComputedRef } from 'vue'

interface UseHistoryReturn {
  /**
   * Save the current state of nodes and edges to the undo stack and reset the redo stack.
   * They are saved as deep copies to prevent mutation issues.
   */
  saveSnapshot: () => void

  /**
   * Revert to the last saved state from the undo stack and push the current state to the redo stack.
   */
  undo: () => void

  /**
   * Reapply the last undone state from the redo stack and push the current state to the undo stack.
   */
  redo: () => void

  /**
   * Indicates whether there are states available to undo.
   */
  canUndo: ComputedRef<boolean>

  /**
   * Indicates whether there are states available to redo.
   */
  canRedo: ComputedRef<boolean>
}

export function useHistory (): UseHistoryReturn {
  const { nodes, edges, setNodes, setEdges } = useVueFlow()

  const undoStack = ref<{ nodes: typeof nodes.value, edges: typeof edges.value }[]>([])
  const redoStack = ref<{ nodes: typeof nodes.value, edges: typeof edges.value }[]>([])
  const canUndo = computed(() => undoStack.value.length > 0)
  const canRedo = computed(() => redoStack.value.length > 0)

  /**
   * Save the current state of nodes and edges to the undo stack and reset the redo stack.
   * They are saved as deep copies to prevent mutation issues.
   */
  function saveSnapshot () {
    undoStack.value.push({
      nodes: JSON.parse(JSON.stringify(nodes.value)),
      edges: JSON.parse(JSON.stringify(edges.value))
    })

    redoStack.value = []
  }

  /**
   * Revert to the last saved state from the undo stack and push the current state to the redo stack.
   */
  function undo () {
    if (undoStack.value.length === 0) {
      return
    }

    const last = undoStack.value.pop()

    redoStack.value.push({
      nodes: JSON.parse(JSON.stringify(nodes.value)),
      edges: JSON.parse(JSON.stringify(edges.value))
    })

    if (!last) {
      return
    }

    setNodes(last.nodes)
    setEdges(last.edges)
  }

  /**
   * Reapply the last undone state from the redo stack and push the current state to the undo stack.
   */
  function redo () {
    if (redoStack.value.length === 0) {
      return
    }

    const next = redoStack.value.pop()

    undoStack.value.push({
      nodes: JSON.parse(JSON.stringify(nodes.value)),
      edges: JSON.parse(JSON.stringify(edges.value))
    })

    if (!next) {
      return
    }

    setNodes(next.nodes)
    setEdges(next.edges)
  }

  return {
    saveSnapshot,
    undo,
    redo,
    canUndo,
    canRedo,
  }
}
```

### useFlowScreenshot [⇧](#table-des-matières)

Ce composable permet d'avoir accès à l'utilitaire de capture d'écran `doScreenshot`.


```js
// use-flow-screenshot.ts

import { useScreenshot } from '@/composables/use-screenshot'
import { useVueFlow } from '@vue-flow/core'

interface UseFlowScreenshotReturn {
  /**
   * Capture a screenshot of the current Vue Flow diagram and download it as an image file.
   * @param fileName Optional base name for the downloaded file. A timestamp will be appended.
   */
  doScreenshot: (fileName?: string) => void
}

export function useFlowScreenshot (): UseFlowScreenshotReturn {
  const { vueFlowRef } = useVueFlow()
  const { capture } = useScreenshot()

  /**
   * Capture a screenshot of the current Vue Flow diagram and download it as an image file.
   * @param fileName Optional base name for the downloaded file. A timestamp will be appended.
   */
  function doScreenshot (fileName?: string) {
    if (!vueFlowRef.value) {
      console.warn('VueFlow element not found')
      return
    }

    capture(vueFlowRef.value, { shouldDownload: true, fileName: `${fileName ?? 'flow'}-screenshot-${Date.now()}` })
  }

  return {
    doScreenshot,
  }
}

```

### useFlowState [⇧](#table-des-matières)

Ce composable servira à gérer l'état du VueFlow via le `localStorage`.

```js
// use-flow-state.ts

import { useToasterStore } from '@/store'
import { type Edge, type Node, useVueFlow } from '@vue-flow/core'

interface UseFlowStateParams {
  /**
   * The initial set of nodes for the flow diagram.
   */
  initialNodes: Node[]

  /**
   * The initial set of edges for the flow diagram.
   */
  initialEdges: Edge[]

  /**
   * Rehydrate nodes to ensure their methods are properly restored.
   * @param nodes The nodes to rehydrate.
   * @returns The rehydrated nodes.
   */
  rehydrateNodes: (nodes: Node[]) => Node[]

  /**
   * Reset the flow diagram to its default state.
   */
  resetFlow: () => void
}

interface UseFlowStateReturn {
  /**
   * Save the current state of nodes and edges to local storage.
   * Also shows a success message.
   * @param prefix The prefix to use for the local storage keys.
   * @param index The index to use for the local storage keys.
   */
  saveCurrentState: (prefix: string, index: string) => void

  /**
   * Restore the state of nodes and edges from local storage.
   * @param prefix The prefix used for the local storage keys.
   * @param index The index used for the local storage keys.
   */
  restoreSavedState: (prefix: string, index: string) => void

  /**
   * Reset the flow diagram to its initial state.
   * Uses the provided rehydration function for nodes.
   * Uses the provided reset function to reset the flow diagram.
   * Uses the initial nodes and edges provided to the composable.
   * Uses the setNodes and setEdges methods from Vue Flow.
   */
  resetToInitialState: () => void
}

/**
 * Composable to manage saving, restoring, and resetting the state of a Vue Flow diagram.
 * @param initialNodes The initial set of nodes for the flow diagram.
 * @param initialEdges The initial set of edges for the flow diagram.
 * @param rehydrateNodes Function to rehydrate nodes to ensure their methods are properly restored.
 * @param resetFlow Function to reset the flow diagram to its default state.
 * @returns The methods to save, restore, and reset the flow state.
 * - `saveCurrentState`: Save the current state of nodes and edges to local storage.
 * - `restoreSavedState`: Restore the state of nodes and edges from local storage.
 * - `resetToInitialState`: Reset the flow diagram to its initial state.
 */
export function useFlowState ({ initialNodes, initialEdges, rehydrateNodes, resetFlow }: UseFlowStateParams): UseFlowStateReturn {
  const { nodes, edges, setNodes, setEdges } = useVueFlow()

  const { addSuccessMessage } = useToasterStore()

  /**
   * Save the current state of nodes and edges to local storage.
   * Also shows a success message.
   * @param prefix The prefix to use for the local storage keys.
   * @param index The index to use for the local storage keys.
   */
  function saveCurrentState (prefix: string, index: string) {
    localStorage.setItem(`${prefix}-flow-nodes-${index}`, JSON.stringify(nodes.value))
    localStorage.setItem(`${prefix}-flow-edges-${index}`, JSON.stringify(edges.value))
    addSuccessMessage({ description: `État sauvegardé avec succès dans l'emplacement ${index}.`, timeout: 2000 })
  }

  /**
   * Restore the state of nodes and edges from local storage.
   * @param prefix The prefix used for the local storage keys.
   * @param index The index used for the local storage keys.
   */
  function restoreSavedState (prefix: string, index: string) {
    const savedNodes = localStorage.getItem(`${prefix}-flow-nodes-${index}`)
    const savedEdges = localStorage.getItem(`${prefix}-flow-edges-${index}`)

    if (!savedNodes?.length || !savedEdges?.length) {
      setNodes(rehydrateNodes(initialNodes))
      setEdges(initialEdges)
      return
    }

    setNodes(rehydrateNodes(JSON.parse(savedNodes)))
    setEdges(JSON.parse(savedEdges))
  }

  /**
   * Reset the flow diagram to its initial state.
   * Uses the provided rehydration function for nodes.
   * Uses the provided reset function to reset the flow diagram.
   * Uses the initial nodes and edges provided to the composable.
   * Uses the setNodes and setEdges methods from Vue Flow.
   */
  function resetToInitialState () {
    setNodes(rehydrateNodes(initialNodes))
    setEdges(initialEdges)
    resetFlow()
  }

  return {
    saveCurrentState,
    restoreSavedState,
    resetToInitialState,
  }
}
```

## Composants utiles [⇧](#table-des-matières)

Afin de simplifier les développements, voilà quelques composants qui pourraient être utiles de garder et d'instancier.

### NodeDropdown [⇧](#table-des-matières)

Ce composant permettra d'avoir accès à différents événements pour chaque noeud. Dans mon exemple il sera possible d'avoir :
- `update` qui permettra de mettre à jour les points d'ancrage du noeud
- `collapse` qui permettra de plier/déplier les enfants du noeud
- `remove` qui permettra de supprimer le noeud

```js
// NodeDropdown.vue

<script setup lang="ts">
import { AvDropdown, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { useI18n } from 'vue-i18n'

/**
 * Props for the NodeDropdown component.
 */
interface NodeDropdownProps {
  /**
   * Indicates whether the children of the node are collapsed.
   */
  collapsed?: boolean

  /**
   * Indicates whether the dropdown includes the option to update the node in the user's profile.
   * @default false
   */
  withProfileUpdate?: boolean
}

const { collapsed, withProfileUpdate = false } = defineProps<NodeDropdownProps>()

/**
 * Emits events related to node dropdown actions.
 * @emits update - When the user wants to update the structure of the node (e.g., its handles).
 * @emits remove - When the user wants to remove the node.
 * @emits collapse - When the user wants to collapse or expand the children of the node.
 * @emits updateInProfile - When the user wants to update the node in their profile and save the data in the API.
 */
const emit = defineEmits<{
  /**
   * Emitted when the user wants to update the structure of the node (e.g., its handles).
   */
  (e: 'update'): void

  /**
   * Emitted when the user wants to remove the node.
   */
  (e: 'remove'): void

  /**
   * Emitted when the user wants to collapse or expand the children of the node.
   */
  (e: 'collapse'): void

  /**
   * Emitted when the user wants to update the node in their profile and save the data in the API.
   * Can only be emitted if `withProfileUpdate` prop is true.
   */
  (e: 'updateInProfile'): void
}>()

const { t } = useI18n()

enum NodeDropdownEvents {
  UPDATE = 'update',
  REMOVE = 'remove',
  COLLAPSE = 'collapse',
  UPDATE_IN_PROFILE = 'updateInProfile',
}

const menuItems = computed(() => [
  {
    name: NodeDropdownEvents.UPDATE,
    icon: MDI_ICONS.PENCIL_OUTLINE,
    label: t('global.buttons.update')
  },
  {
    name: NodeDropdownEvents.REMOVE,
    icon: MDI_ICONS.TRASH_CAN_OUTLINE,
    label: t('global.buttons.delete')
  },
  {
    name: NodeDropdownEvents.COLLAPSE,
    icon: collapsed ? MDI_ICONS.PLUS : MDI_ICONS.MINUS,
    label: collapsed ? t('global.buttons.expand') : t('global.buttons.collapse'),
  },
  ...(withProfileUpdate
    ? [{
        name: NodeDropdownEvents.UPDATE_IN_PROFILE,
        icon: MDI_ICONS.TRAY_UPLOAD,
        label: t('global.vueFlow.NodeDropdown.updateInProfile')
      }]
    : []),
])

function handleItemSelected (itemName: string) {
  switch (itemName) {
    case NodeDropdownEvents.UPDATE:
      emit('update')
      break
    case NodeDropdownEvents.REMOVE:
      emit('remove')
      break
    case NodeDropdownEvents.COLLAPSE:
      emit('collapse')
      break
    case NodeDropdownEvents.UPDATE_IN_PROFILE:
      withProfileUpdate && emit('updateInProfile')
      break
  }
}
</script>

<template>
  <div class="node-dropdown-container">
    <AvDropdown
      :items="menuItems"
      trigger-aria-label="Paramètres du noeud lien"
      :trigger-icon="MDI_ICONS.SETTINGS"
      trigger-small
      width="max-content"
      @item-selected="handleItemSelected"
    />
  </div>
</template>

<style lang="scss" scoped>
.node-dropdown-container {
  position: absolute;
  top: -1.75rem;
  right: var(--spacing-none);
}
</style>
```

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_node_dropdown.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : Dropdown de paramétrage d'un noeud</figcaption>
</figure>
<br />


### Handles [⇧](#table-des-matières)

Ce composant permet de placer 8 `Handle` de VueFlow dans le noeud qui sera défini. Il y a deux `Handle` par position (`top`, `right`, `bottom`, `left`), un `source` et un `target`. Cela permettra de paramétrer les handles de chaque position.

```js
// Handles.vue

<script setup lang="ts">
import { Handle, type NodeProps, Position } from '@vue-flow/core'

defineProps<Pick<NodeProps, 'data'>>()
</script>

<template>
  <div>
    <Handle
      id="source-top"
      type="source"
      :position="Position.Top"
      :connectable="data.top === 'source'"
      :style="{ opacity: data.top === 'source' ? 1 : 0 }"
    />
    <Handle
      id="target-top"
      type="target"
      :position="Position.Top"
      :connectable="data.top === 'target'"
      :style="{ opacity: data.top === 'target' ? 1 : 0 }"
    />

    <Handle
      id="source-right"
      type="source"
      :position="Position.Right"
      :connectable="data.right === 'source'"
      :style="{ opacity: data.right === 'source' ? 1 : 0 }"
    />
    <Handle
      id="target-right"
      type="target"
      :position="Position.Right"
      :connectable="data.right === 'target'"
      :style="{ opacity: data.right === 'target' ? 1 : 0 }"
    />

    <Handle
      id="source-bottom"
      type="source"
      :position="Position.Bottom"
      :connectable="data.bottom === 'source'"
      :style="{ opacity: data.bottom === 'source' ? 1 : 0 }"
    />
    <Handle
      id="target-bottom"
      type="target"
      :position="Position.Bottom"
      :connectable="data.bottom === 'target'"
      :style="{ opacity: data.bottom === 'target' ? 1 : 0 }"
    />

    <Handle
      id="source-left"
      type="source"
      :position="Position.Left"
      :connectable="data.left === 'source'"
      :style="{ opacity: data.left === 'source' ? 1 : 0 }"
    />
    <Handle
      id="target-left"
      type="target"
      :position="Position.Left"
      :connectable="data.left === 'target'"
      :style="{ opacity: data.left === 'target' ? 1 : 0 }"
    />
  </div>
</template>
```

#### Handles - version simplifiée [⇧](#table-des-matières)

Dans un cas où il n'y aurait pas de distinction entre `source` et `target` pour les points d'ancrage des liens, la simplification suivante peut être réalisée.

```js
// Handles.vue

<script setup lang="ts">
import { Handle, type NodeProps, Position } from '@vue-flow/core'

defineProps<Pick<NodeProps, 'data'>>()

const handles = Object.values(Position)
</script>

<template>
  <div>
    <Handle
      v-for="handle in handles"
      :id="handle"
      :key="handle"
      :position="handle"
      :connectable="data[handle] === true"
      :style="{ opacity: data[handle] === true ? 1 : 0 }"
    />
  </div>
</template>
```

### UpdateHandleSelector [⇧](#table-des-matières)

Afin de préparer la modale de mise à jour des handles et ne pas répéter le code, voilà un composant utilisant `AvRadioButtonSet` réutilisable :

```js
// UpdateHandleSelector.vue

<script setup lang="ts">
import { HandleType } from '@/types/handle.types'
import { AvIconText, AvRadioButton, AvRadioButtonSet, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { Position } from '@vue-flow/core'

interface UpdateHandleSelectorProps {
  position: Position
}

const { position } = defineProps<UpdateHandleSelectorProps>()

const emit = defineEmits<{
  (e: 'update:model-value', value: HandleType): void
}>()

const modelValue = ref<HandleType>(HandleType.NONE)

const ALLOWED_VALUES = [
  { value: HandleType.NONE, label: 'Aucun', icon: MDI_ICONS.CLOSE_CIRCLE_OUTLINE },
  { value: HandleType.SOURCE, label: 'Source', icon: MDI_ICONS.CIRCLE },
  { value: HandleType.TARGET, label: 'Cible', icon: MDI_ICONS.CIRCLE_OUTLINE }
]

const legend = computed(() => {
  switch (position) {
    case Position.Top:
      return 'Connecteur du haut'
    case Position.Right:
      return 'Connecteur de droite'
    case Position.Bottom:
      return 'Connecteur du bas'
    case Position.Left:
      return 'Connecteur de gauche'
    default:
      return ''
  }
})

const inline = computed(() => position === Position.Top || position === Position.Bottom)

function updateModelValue (value: string | number | boolean) {
  modelValue.value = value as HandleType
  emit('update:model-value', modelValue.value)
}
</script>

<template>
  <div :class="{ 'av-row av-row--center': inline }">
    <AvRadioButtonSet
      :model-value="modelValue"
      :legend="legend"
      :name="`${position.toLowerCase()}-handle`"
      :inline="inline"
      small
      @update:model-value="updateModelValue"
    >
      <template
        v-for="value in ALLOWED_VALUES"
        :key="value.value"
      >
        <AvRadioButton :value="value.value">
          <AvIconText
            v-if="value.value !== HandleType.NONE"
            :icon="value.icon"
            :text="value.label"
            typography-class="caption-regular"
            gap="var(--spacing-xs)"
            icon-color="var(--dark-background-primary1)"
          />
          <span
            v-else
            class="caption-regular"
          >{{ value.label }}</span>
        </AvRadioButton>
      </template>
    </AvRadioButtonSet>
  </div>
</template>

<style lang="scss" scoped>
:deep(.icon-text--container) {
  flex-direction: row-reverse;
}
</style>
```

#### UpdateHandleSelector - version simplifiée [⇧](#table-des-matières)

Dans un cas où il n'y aurait pas de distinction entre `source` et `target` pour les points d'ancrage des liens, la simplification suivante peut être réalisée.

```js
// UpdateHandleSelector.vue

<script setup lang="ts">
import Toggle from '@/common/components/Toggle/Toggle.vue'
import { useAvBreakpoints } from '@avenirs-esr/avenirs-dsav'
import { Position } from '@vue-flow/core'
import { useI18n } from 'vue-i18n'

/**
 * Props for the UpdateHandleSelector component.
 */
interface UpdateHandleSelectorProps {
  /**
   * Indicates whether the handle is enabled or not.
   */
  modelValue?: boolean

  /**
   * The position of the handle.
   */
  position: Position
}

const { position, modelValue } = defineProps<UpdateHandleSelectorProps>()

/**
 * Emits events related to updating the handle selector.
 * @emits update:model-value - When the model value (enabled/disabled) of the handle changes.
 */
defineEmits<{
  /**
   * Emitted when the model value (enabled/disabled) of the handle changes.
   * @param value - The new model value.
   */
  (e: 'update:model-value', value: boolean): void
}>()

const { t } = useI18n()
const { isMobile } = useAvBreakpoints()

const description = computed(() => t(`global.vueFlow.UpdateHandleSelector.position.${position}`))

const inline = computed(() => position === Position.Top || position === Position.Bottom)
</script>

<template>
  <div
    :class="{ 'av-row av-row--center': inline,
              'is-mobile': isMobile,
    }"
  >
    <Toggle
      :model-value="modelValue"
      :description="description"
      @update:model-value="$emit('update:model-value', $event as boolean)"
    />
  </div>
</template>

<style lang="scss" scoped>
.is-mobile {
  :deep(.av-toggle, .toggle) {
    flex-direction: column;
    align-items: center;
  }
}
</style>
```

### UpdateHandlesModal [⇧](#table-des-matières)

Ce composant permettra de mettre à jour les handles des noeuds (et par conséquent les noeuds) en renseignant pour chaque position (`top`, `right`, `bottom`, `left`) le type de handle voulu (`source` ou `target`) ou s'il doit ête masqué (`none`).

```js
// UpdateHandlesModal.vue

<script setup lang="ts">
import ConfirmationModal from '@/components/ConfirmationModal.vue'
import UpdateHandleSelector from '@/components/UpdateHandleSelector.vue'
import { HandleType } from '@/types/handle.types'
import { AvCard, AvIcon, MDI_ICONS } from '@avenirs-esr/avenirs-dsav'
import { type NodeProps, Position, useVueFlow } from '@vue-flow/core'

interface UpdateHandlesModalProps extends Pick<NodeProps, 'id' | 'data'> {
  show: boolean
}

const { id, data, show } = defineProps<UpdateHandlesModalProps>()

const emit = defineEmits<{
  (e: 'close'): void
}>()

const { updateNode } = useVueFlow()

const topModelValue = ref<HandleType>(data.top)
const rightModelValue = ref<HandleType>(data.right)
const bottomModelValue = ref<HandleType>(data.bottom)
const leftModelValue = ref<HandleType>(data.left)

function onConfirm () {
  updateNode(id, {
    data: {
      ...data,
      top: topModelValue.value,
      right: rightModelValue.value,
      bottom: bottomModelValue.value,
      left: leftModelValue.value
    }
  })
  emit('close')
}

function getIconVisibility (handleType: HandleType) {
  return handleType !== HandleType.NONE
}

function getHandleIcon (handleType: HandleType) {
  return handleType === HandleType.SOURCE ? MDI_ICONS.CIRCLE : MDI_ICONS.CIRCLE_OUTLINE
}
</script>

<template>
  <ConfirmationModal
    :show="show"
    @close="$emit('close')"
    @confirm="onConfirm"
  >
    <div class="av-flex-col-sm">
      <UpdateHandleSelector
        v-model="topModelValue"
        :position="Position.Top"
      />

      <div class="av-flex-row-sm av-row--middle av-row av-row--center">
        <UpdateHandleSelector
          v-model="leftModelValue"
          :position="Position.Left"
        />

        <div class="handles-container">
          <div class="top-handle">
            <AvIcon
              v-if="getIconVisibility(topModelValue)"
              :name="getHandleIcon(topModelValue)"
              color="var(--dark-background-primary1)"
            />
          </div>

          <div class="right-handle">
            <AvIcon
              v-if="getIconVisibility(rightModelValue)"
              :name="getHandleIcon(rightModelValue)"
              color="var(--dark-background-primary1)"
            />
          </div>

          <div class="bottom-handle">
            <AvIcon
              v-if="getIconVisibility(bottomModelValue)"
              :name="getHandleIcon(bottomModelValue)"
              color="var(--dark-background-primary1)"
            />
          </div>

          <div class="left-handle">
            <AvIcon
              v-if="getIconVisibility(leftModelValue)"
              :name="getHandleIcon(leftModelValue)"
              color="var(--dark-background-primary1)"
            />
          </div>
          <AvCard>
            <template #title>
              Aperçu du noeud
            </template>
            <div class="demo-card-content" />
          </AvCard>
        </div>

        <UpdateHandleSelector
          v-model="rightModelValue"
          :position="Position.Right"
        />
      </div>

      <UpdateHandleSelector
        v-model="bottomModelValue"
        :position="Position.Bottom"
      />
    </div>
  </ConfirmationModal>
</template>

<style lang="scss" scoped>
.demo-card-content {
  width: 300px;
}

.handles-container {
  position: relative;
  height: 100%;

  .top-handle,
  .right-handle,
  .bottom-handle,
  .left-handle {
    position: absolute;
  }

  .top-handle {
    top: -0.5rem;
    left: calc(50% - 12px);
  }

  .right-handle {
    top: calc(50% - 12px);
    right: -0.5rem;
  }

  .bottom-handle {
    bottom: -0.5rem;
    left: calc(50% - 12px);
  }

  .left-handle {
    top: calc(50% - 12px);
    left: -0.5rem;
  }
}
</style>
```

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_update_handles_modal.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : Modale de à jour des points d'acrange des noeuds</figcaption>
</figure>
<br />

#### UpdateHandlesModal - version simplifiée [⇧](#table-des-matières)

Dans un cas où il n'y aurait pas de distinction entre `source` et `target` pour les points d'ancrage des liens, la simplification suivante peut être réalisée.

```js
<script setup lang="ts">
import ConfirmationModal from '@/common/components/ConfirmationModal/ConfirmationModal.vue'
import UpdateHandleSelector from '@/common/components/VueFlow/UpdateHandleSelector/UpdateHandleSelector.vue'
import { AvCard, AvIcon, MDI_ICONS, useAvBreakpoints } from '@avenirs-esr/avenirs-dsav'
import { type NodeProps, Position, useVueFlow } from '@vue-flow/core'
import { useI18n } from 'vue-i18n'

interface UpdateHandlesModalProps extends Pick<NodeProps, 'id' | 'data'> {
  show: boolean
}

const { id, data, show } = defineProps<UpdateHandlesModalProps>()

const emit = defineEmits<{
  (e: 'close'): void
}>()

const { t } = useI18n()
const { updateNode } = useVueFlow()
const { isMobile } = useAvBreakpoints()

const topModelValue = ref<boolean | undefined>(data.top)
const rightModelValue = ref<boolean | undefined>(data.right)
const bottomModelValue = ref<boolean | undefined>(data.bottom)
const leftModelValue = ref<boolean | undefined>(data.left)

function onConfirm () {
  updateNode(id, {
    data: {
      ...data,
      top: topModelValue.value,
      right: rightModelValue.value,
      bottom: bottomModelValue.value,
      left: leftModelValue.value
    }
  })
  emit('close')
}

function getIconVisibility (handleType: boolean | undefined) {
  return handleType === true
}

const handleIcons = [
  { position: Position.Top, modelValue: topModelValue },
  { position: Position.Right, modelValue: rightModelValue },
  { position: Position.Bottom, modelValue: bottomModelValue },
  { position: Position.Left, modelValue: leftModelValue }
]
</script>

<template>
  <ConfirmationModal
    :show="show"
    @close="$emit('close')"
    @confirm="onConfirm"
  >
    <template #header>
      <div class="av-row av-row--center">
        <span class="b2-bold">{{ t('global.vueFlow.UpdateHandlesModal.title') }}</span>
      </div>
    </template>
    <div class="av-flex-col-sm">
      <UpdateHandleSelector
        :model-value="topModelValue"
        :position="Position.Top"
        @update:model-value="(value) => topModelValue = value"
      />

      <div class="av-flex-row-sm av-row--middle av-row av-row--center">
        <UpdateHandleSelector
          v-model="leftModelValue"
          :position="Position.Left"
        />

        <div class="handles-container">
          <div
            v-for="icon in handleIcons"
            :key="icon.position"
            :class="`${icon.position}-handle`"
          >
            <AvIcon
              v-if="getIconVisibility(icon.modelValue.value)"
              :name="MDI_ICONS.CIRCLE"
              color="var(--dark-background-primary1)"
            />
          </div>

          <AvCard>
            <template #title>
              <span class="b2-bold">{{ t('global.vueFlow.UpdateHandlesModal.preview') }}</span>
            </template>
            <div
              class="demo-card-content"
              :class="{ 'demo-card-content--mobile': isMobile }"
            />
          </AvCard>
        </div>

        <UpdateHandleSelector
          v-model="rightModelValue"
          :position="Position.Right"
        />
      </div>

      <UpdateHandleSelector
        v-model="bottomModelValue"
        :position="Position.Bottom"
      />
    </div>
  </ConfirmationModal>
</template>

<style lang="scss" scoped>
.demo-card-content {
  width: 300px;

  &--mobile {
    width: 150px;
  }
}

.handles-container {
  position: relative;
  height: 100%;

  .top-handle,
  .right-handle,
  .bottom-handle,
  .left-handle {
    position: absolute;
  }

  .top-handle {
    top: -0.5rem;
    left: calc(50% - 12px);
  }

  .right-handle {
    top: calc(50% - 12px);
    right: -0.5rem;
  }

  .bottom-handle {
    bottom: -0.5rem;
    left: calc(50% - 12px);
  }

  .left-handle {
    top: calc(50% - 12px);
    left: -0.5rem;
  }
}
</style>
```

<figure>
  <video controls style="width: 50%; height: auto;">
    <source src="{{ '/assets/videos/vueflow/vueflow_update_handles_modal_simple.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption>Démonstration VueFlow : Modale de à jour des points d'acrange des noeuds - version simplifiée</figcaption>
</figure>
<br />

### NodeTemplate [⇧](#table-des-matières)

Ce composant permettra d'avoir un template commun à tous les noeuds afin d'avoir une base de fonctionnement (`NodeDropdown`) et de style (`AvCard`) communes à tous les noeuds.

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import type { AvCardProps } from 'node_modules/@avenirs-esr/avenirs-dsav/dist/components/cards/AvCard/AvCard.vue'
import type { Slot } from 'vue'
import Handles from '@/common/components/VueFlow/Handles/Handles.vue'
import NodeDropdown from '@/common/components/VueFlow/NodeDropdown/NodeDropdown.vue'
import UpdateHandlesModal from '@/common/components/VueFlow/UpdateHandlesModal/UpdateHandlesModal.vue'
import { useModal } from '@/common/composables'
import { useNodes } from '@/common/composables/VueFlow/use-nodes/use-nodes'
import { AvCard } from '@avenirs-esr/avenirs-dsav'

/**
 * Props for the NodeTemplate component.
 */
export interface NodeTemplateProps extends NodeProps, AvCardProps {
  /**
   * If true, the dropdown menu will not be displayed.
   */
  withoutDropdown?: boolean

  /**
   * If true, the card will be displayed in title-only mode.
   */
  titleOnly?: boolean

  /**
   * Indicates whether the dropdown includes the option to update the node in the user's profile.
   */
  withProfileUpdate?: boolean
}

defineProps<NodeTemplateProps>()

/**
 * Emits for the NodeTemplate component.
 * @emit remove - Emitted when the node is removed.
 * @emit updateInProfile - Emitted when the node is updated in the profile.
 */
const emit = defineEmits<{
  /**
   * Emitted when the node is removed.
   * @param id - The ID of the node to remove.
   */
  (e: 'remove', id: string): void

  /**
   * Emitted when the user wants to update the node in the profile and in the API.
   */
  (e: 'updateInProfile'): void
}>()

/**
 * Slots for the NodeTemplate component.
 * @slot title - Slot for the title content.
 * @slot default - Default slot for the main content.
 */
defineSlots<{
  /**
   * Slot for the title content.
   */
  title: Slot

  /**
   * Default slot for the main content.
   */
  default: Slot
}>()

const { removeNodeWithChildren, toggle } = useNodes()
const { displayModal, hideModal, showModal } = useModal()

function removeNodeHandler (id: string) {
  emit('remove', id)
  removeNodeWithChildren(id)
}
</script>

<template>
  <div class="node-container">
    <AvCard
      :class="{ 'av-card--title-only': titleOnly }"
      v-bind="$props"
    >
      <template #title>
        <div class="av-row av-flex-row-sm av-row--between av-row--middle">
          <slot name="title" />
        </div>
      </template>

      <slot />
    </AvCard>

    <Handles :data="data" />

    <NodeDropdown
      v-if="!withoutDropdown"
      :collapsed="data.collapsed"
      :with-profile-update="withProfileUpdate"
      @update="displayModal"
      @collapse="() => toggle(id)"
      @remove="() => removeNodeHandler(id)"
      @update-in-profile="$emit('updateInProfile')"
    />

    <UpdateHandlesModal
      :id="id"
      :show="showModal"
      :data="data"
      @close="hideModal"
    />
  </div>
</template>

<style lang="scss" scoped>
.node-container {
  position: relative;
}

:deep(.av-card--title-only) {
  .av-card__title {
    margin: calc(var(--spacing-sm) * -1);
  }
  .av-card__content-collapsible {
    padding: var(--spacing-none);
  }
}
</style>
```

### ButonNodeTemplate [⇧](#table-des-matières)

Ce template servira principalement pour des noeuds permettant des actions, comme par exemple ajouter un noeud enfant à un noeud.

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import NodeTemplate from '@/components/NodeTemplate.vue'
import { AvButton, type AvButtonProps } from '@avenirs-esr/avenirs-dsav'

/**
 * Props for the ButtonNodeTemplate component.
 * Extends NodeProps and AvButtonProps.
 * Uses label from AvButtonProps.
 */
interface ButtonNodeTemplateProps extends NodeProps, AvButtonProps {
  label: AvButtonProps['label']
}

defineProps<ButtonNodeTemplateProps>()

/**
 * Emits for the ButtonNodeTemplate component.
 * @emit click - Emitted when the button is clicked.
 */
defineEmits<{
  /**
   * Emitted when the button is clicked.
   */
  (e: 'click'): void
}>()
</script>

<template>
  <NodeTemplate
    v-bind="$props"
    title-only
    without-dropdown
    title-background="var(--other-background-base)"
  >
    <template #title>
      <AvButton
        :label="label"
        :icon="icon"
        :small="small"
        icon-only
        @click.stop="$emit('click')"
      />
    </template>

    <slot />
  </NodeTemplate>
</template>

<style lang="scss" scoped>
.s1-bold {
  color: var(--text2);
}

:deep(.av-card) {
  padding: 0.25rem;
  border-radius: 0.5rem;
}
</style>
```

### TitleDescriptionNodeTemplate [⇧](#table-des-matières)

Ce template permettra d'avoir accès à des noeuds simples constitués d'un titre dans le slot `title` de `AvCard` et d'une description (+ un contenu supplémentaire via le slot `default`) dans le slot `default` de `AvCard`.

```js
<script setup lang="ts">
import type { NodeProps } from '@vue-flow/core'
import NodeTemplate from '@/features/PocVueFlow/global/components/NodeTemplate.vue'

defineProps<NodeProps>()
</script>

<template>
  <NodeTemplate
    v-bind="$props"
    title-only
    title-background="var(--other-background-base)"
  >
    <template #title>
      <span class="b2-bold">{{ data.title }}</span>
    </template>

    <span class="caption-regular">{{ data.description }}</span>

    <slot />
  </NodeTemplate>
</template>
```