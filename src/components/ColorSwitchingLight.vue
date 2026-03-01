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
    <TresMeshStandardMaterial
      :color="isOn ? colorOn : colorOff"
      :emissive="isOn ? colorOn : colorOff"
      :emissiveIntensity="1"
      :metalness="0.2"
      :roughness="0.8"
    />
    <!-- <TresDirectionalLight :position="[-4,8,5]" color="red" :intensity="2" /> -->
    <!--<TresPointLight 
      :position="[pos_x, pos_y, pos_z]" 
      :intensity="isOn ? 10.8 : 0.4" 
      cast-shadow 
      :color = "isOn ? colorOn : colorOff"
    />-->
    <!--<TresRectAreaLight
      :position="[pos_x, pos_y, pos_z]" 
      :intensity="isOn ? 2.8 : 0.4" 
      :width="1.0"
      :height="1.0"
      :look-at="[0, 0, 0]"
      cast-shadow 
      :color = "isOn ? colorOn : colorOff"
    />-->
  </TresMesh>
</template>
