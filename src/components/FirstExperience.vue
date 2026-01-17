<script setup lang="ts">
// No imports needed! TresJS components are available globally
  import { useLoop } from '@tresjs/core';
  import { ref } from 'vue';

  const donutRef = ref();

  const { onBeforeRender } = useLoop();

  onBeforeRender(({ elapsed }) => {
    if (donutRef.value) {
      donutRef.value.rotation.x = elapsed * 0.9;
      donutRef.value.rotation.y = elapsed * 0.8;
    }
  });
</script>

<template>
  <!-- Camera Setup -->
  <TresPerspectiveCamera
    :position="[7, 7, 7]"
    :look-at="[0, 0, 0]"
  />

  <TresMesh ref="donutRef" :position="[0, 2, 0]">
    <TresTorusGeometry :args="[1, 0.4, 16, 60]" />
    <TresMeshBasicMaterial color="#ff6b35" />
  </TresMesh>

  <TresAxesHelper />
  <TresGridHelper />
</template>

