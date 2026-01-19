<script setup lang="ts">
// No imports needed! TresJS components are available globally
  import { useLoop } from '@tresjs/core';
  import { ref } from 'vue';

  const props = defineProps({
    pos_x: {
      type: Number,
      required: true,
    },
    pos_y: {
      type: Number,
      required: true,
    },
    pos_z: {
      type: Number,
      required: true,
    },
    colorOn: {
      type: String,
      required: true,
    },
    colorOff: {
      type: String,
      required: true,
    },
    isOn: {
        type: Boolean,
        default: false,
    }
  });

  const emit = defineEmits(['toggle']);
</script>

<template>
  <TresMesh
    :position="[pos_x, pos_y, pos_z]"
    @click="emit('toggle')"
  >
    <TresBoxGeometry :args="[1, 1, 1]" />
    <TresMeshStandardMaterial />
    <!-- <TresDirectionalLight :position="[-4,8,5]" color="red" :intensity="2" /> -->
    <TresPointLight 
      :position="[pos_x, pos_y, pos_z]" 
      :intensity="isOn ? 4.8 : 4.2" 
      cast-shadow 
      :color = "isOn ? colorOn : colorOff"
    />
  </TresMesh>
</template>
