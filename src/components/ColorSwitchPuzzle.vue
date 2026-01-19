<script setup lang="ts">
// No imports needed! TresJS components are available globally
  import { useLoop } from '@tresjs/core';
  import { ref } from 'vue';
  import ColorSwitchingLight from './ColorSwitchingLight.vue';

  const props = defineProps({
    puzzleSetup: {
      type: String,
      required: true,
    },
    colorOn: {
      type: String,
      required: true,
    },
    colorOff: {
      type: String,
      required: true,
    }
  });
  

    // <li v-for="i in lights">
    //     <ColorSwitchingLight 
    //         :pos_x="{{ i.x }}" 
    //         :pos_y="{{ i.y }}" 
    //         :pos_z="{{ i.z }}" 
    //         :colorOn="{{ props.colorOn }}" 
    //         :colorOff="{{ props.colorOff }}" 
    //         :isOn="{{ i.isOn }}"
    //     />
    // </li> 

  const length = props.puzzleSetup.length;
  const dim = Math.sqrt(length);

  //const lights = ref([]);
  const lights = [];

  for(let i = 0; i < length; i++) {
    const x = i % dim;
    const y = Math.floor(i / dim);
    console.log(`Index: ${i}, X: ${x}, Y: ${y}, Value: ${props.puzzleSetup[i]}`);
    lights.push({
      pos_x: x * 2 - (dim - 1),
      pos_y: 1,
      pos_z: y * 2 - (dim - 1),
      isOn: props.puzzleSetup[i] === '1'
    });
  }

</script>

<template>
    <ColorSwitchingLight
        v-for="i in lights"
        :key="item.id"
        :item="item"
        :pos_x="i.x" 
        :pos_y="i.y" 
        :pos_z="i.z" 
        :colorOn="props.colorOn" 
        :colorOff="props.colorOff" 
        :isOn="i.isOn"
    />
</template>