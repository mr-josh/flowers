<!--
Auto-generated by: https://github.com/threlte/threlte/tree/main/packages/gltf
Command: npx @threlte/gltf@2.0.1 /Users/mr-josh/Documents/mr-josh/flowers/static/models/shrub.glb --root /models/ --types --printwidth 120 --precision 2
-->

<script lang="ts">
  import type * as THREE from "three";
  import { Group, MeshToonMaterial } from "three";
  import { T, type Props, type Events, type Slots, forwardEventHandlers } from "@threlte/core";
  import { useGltf } from "@threlte/extras";

  type $$Props = Props<THREE.Group> & { opacity?: number };
  type $$Events = Events<THREE.Group>;
  type $$Slots = Slots<THREE.Group> & { fallback: {}; error: { error: any } };

  export const ref = new Group();
  export let opacity: number = 1;

  type GLTFResult = {
    nodes: {
      Leaf000: THREE.Mesh;
      Leaf001: THREE.Mesh;
      Leaf002: THREE.Mesh;
      Leaf003: THREE.Mesh;
      Leaf004: THREE.Mesh;
    };
    materials: {
      Leaf: THREE.MeshStandardMaterial;
    };
  };

  const gltf = useGltf<GLTFResult>("/models/shrub.glb");

  const component = forwardEventHandlers();
  const material = new MeshToonMaterial({ color: 0x77ff77, opacity, transparent: true });

  $: material.opacity = opacity;
</script>

<T is={ref} dispose={false} {...$$restProps} bind:this={$component}>
  {#await gltf}
    <slot name="fallback" />
  {:then gltf}
    <T.Group scale={0.5}>
      <T.Mesh
        geometry={gltf.nodes.Leaf000.geometry}
        {material}
        position={[-0.46, 0.39, 0]}
        rotation={[0.32, -0.02, 0.16]}
      />
      <T.Mesh
        geometry={gltf.nodes.Leaf001.geometry}
        {material}
        position={[-0.05, 0.54, 0.07]}
        rotation={[0.09, -0.18, -0.59]}
      />
      <T.Mesh
        geometry={gltf.nodes.Leaf002.geometry}
        {material}
        position={[0.29, 0.3, -0.12]}
        rotation={[-0.37, -0.18, -1.66]}
      />
      <T.Mesh
        geometry={gltf.nodes.Leaf003.geometry}
        {material}
        position={[0.13, 0.55, -0.37]}
        rotation={[-0.06, -0.16, -0.96]}
      />
      <T.Mesh
        geometry={gltf.nodes.Leaf004.geometry}
        {material}
        position={[-0.39, 0.53, -0.32]}
        rotation={[-0.06, -0.23, -0.28]}
      />
    </T.Group>
  {:catch error}
    <slot name="error" {error} />
  {/await}

  <slot {ref} />
</T>
