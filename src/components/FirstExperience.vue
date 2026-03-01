<script setup lang="ts">
// No imports needed! TresJS components are available globally
  import { useLoop } from '@tresjs/core';
  import { ref } from 'vue';

  import TheDonut from './TheDonut.vue';

  import { EffectComposer,EffectBloom } from '@tresjs/post-processing';

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

  <TheDonut /> 


  <TresAxesHelper />
  <TresGridHelper />
</template>

