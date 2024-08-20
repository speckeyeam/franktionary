<script>
  import { Group } from "three";
  import { T, forwardEventHandlers } from "@threlte/core";
  import { useGltf } from "@threlte/extras";
  import { TextureLoader } from "three";
  import { useLoader } from "@threlte/core";

  export const ref = new Group();

  const gltf = useGltf("models/cookie.glb");
  const texture = useLoader(TextureLoader).load("models/cookie.jpg");

  const component = forwardEventHandlers();
</script>

<T is={ref} dispose={false} {...$$restProps} bind:this={$component}>
  {#await gltf}
    <slot name="fallback" />
  {:then gltf}
    {(gltf.materials.Material.roughness = 1)}
    {(gltf.materials.Material.metalness = 0)}

    <T.AmbientLight intensity={0.5} />
    <T.DirectionalLight position={[10, 10, 10]} intensity={1.5} />
    <T.Mesh
      geometry={gltf.nodes.Cube.geometry}
      material={gltf.materials.Material}
    />

    {#if $texture}{/if}
  {:catch error}
    <slot name="error" {error} />
  {/await}

  <slot {ref} />
</T>
