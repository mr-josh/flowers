<script lang="ts">
  import font from "@fontsource/coming-soon/files/coming-soon-latin-400-normal.woff";
  import { beforeUpdate, onMount } from "svelte";
  import { T } from "@threlte/core";
  import { ContactShadows, Grid, Text } from "@threlte/extras";
  import Flower from "./models/flower.svelte";
  import Shrub from "./models/shrub.svelte";

  let scroll = 0;
  let isDragging = false;
  let dragVelocity = 0;
  let dragStart = -1;

  onMount(() => {
    const interval = setInterval(() => {
      if (isDragging || Math.abs(dragVelocity) <= 0) return;
      dragVelocity *= 0.9;
      scroll += dragVelocity;
    }, 1000 / 60);

    return () => {
      clearInterval(interval);
    };
  });

  beforeUpdate(() => {
    if (scroll > 0) scroll = 0;
  });

  const falloff = 40;
  const backline = 14;
  let shrubs = [{ position: [0, 0, 0], rotation: 0 }];
  let flowers = [{ position: [0, 0, 0], rotation: 0, color: 0 }];

  for (let i = 0; i < 70; i++) {
    shrubs.push({
      position: [Math.random() * 20 - 10, 0, Math.random() * falloff],
      rotation: Math.random() * (Math.PI / 2) - Math.PI / 4,
    });
    flowers.push({
      position: [Math.random() * 20 - 10, 0, Math.random() * falloff],
      rotation: Math.random() * (Math.PI / 2) - Math.PI / 4,
      color: Math.floor(Math.random() * 180),
    });
  }
  shrubs = [...shrubs];
  flowers = [...flowers];
</script>

<!-- #region window -->
<svelte:window
  on:wheel={({ deltaY }) => {
    scroll -= deltaY / 100;
  }}
  on:touchstart={() => {
    isDragging = true;
  }}
  on:touchmove={({ touches }) => {
    if (isDragging) {
      const touch = touches[0];
      if (dragStart === -1) {
        dragStart = touch.clientY;
      }
      dragVelocity = (touch.clientY - dragStart) / 20;
      dragStart = touch.clientY;
      scroll += dragVelocity;
    }
  }}
  on:touchend={() => {
    isDragging = false;
    dragStart = -1;
  }}
/>
<!-- #endregion window -->

<!-- #region gizmos -->
<T.PerspectiveCamera
  makeDefault
  position={[0, 5, 15]}
  fov={45}
  on:create={({ ref }) => {
    ref.lookAt(0, 0, 0);
  }}
/>
<T.DirectionalLight intensity={0.8} position.x={7} position.y={10} />
<T.AmbientLight intensity={0.2} />
<ContactShadows scale={20} blur={2} far={2.5} opacity={0.5} />

<Grid
  position.y={-0.001}
  position.z={scroll % 2}
  cellColor="#eeffee"
  sectionColor="#eeffee"
  sectionThickness={0}
  fadeDistance={30}
  cellSize={2}
  gridSize={30}
/>
<!-- #endregion Scene -->

<T.Group position.z={scroll}>
  <Text
    text={"Happy valentines day!"}
    position={[0, 2.5, 0]}
    {font}
    fontSize={0.65}
    anchorX="50%"
    color="#ffaaee"
    fillOpacity={3 + scroll * 0.25}
  />
  <Text
    text={"Each flower here is to"}
    position={[0, 1.75, 2.2]}
    {font}
    fontSize={0.5}
    anchorX="50%"
    color="#ffffff"
    fillOpacity={4 + scroll * 0.25}
  />
  <Text
    text={"remind you that you are loved"}
    position={[0, 1, 3]}
    {font}
    fontSize={0.5}
    anchorX="50%"
    color="#ffffff"
    fillOpacity={5 + scroll * 0.25}
  />
  <Text
    text={"(scroll to see all the flowers)"}
    position={[0, 0.5, 3.5]}
    {font}
    fontSize={0.25}
    anchorX="50%"
    color="#ffffff"
    fillOpacity={5.2 + scroll * 0.25}
  />
</T.Group>

<!-- #region shrubs -->
<T.Group position.z={14}>
  {#each shrubs as shrub}
    <Shrub
      position.x={shrub.position[0]}
      position.y={shrub.position[1]}
      position.z={(shrub.position[2] + scroll) % falloff}
      rotation.y={shrub.rotation}
      opacity={Math.min(1, backline + ((shrub.position[2] + scroll) % falloff) * 0.5)}
    />
  {/each}
</T.Group>
<!-- #endregion shrubs -->

<!-- #region flowers -->
<T.Group position.z={14}>
  {#each flowers as flower}
    <Flower
      position.x={flower.position[0]}
      position.y={flower.position[1]}
      position.z={(flower.position[2] + scroll) % falloff}
      rotation.y={flower.rotation}
      opacity={Math.min(1, backline + ((flower.position[2] + scroll) % falloff) * 0.5)}
      color={flower.color}
    />
  {/each}
</T.Group>
<!-- #endregion flowers -->
