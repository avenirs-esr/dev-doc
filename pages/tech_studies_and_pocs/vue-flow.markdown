---
layout: page
title: Documentation VueFlow
permalink: /vue-flow/
---

_Last updated: 2025-12-05_

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