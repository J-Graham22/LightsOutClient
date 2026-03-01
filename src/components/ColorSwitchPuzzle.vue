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
  const lights = ref<{
    pos_x: Number,
    pos_y: Number,
    pos_z: Number,
    isOn: Boolean
  }[]>
  ([]);

  for(let i = 0; i < length; i++) {
    const x: Number = i % dim;
    const y: Number = Math.floor(i / dim);
    console.log(`Index: ${i}, X: ${x}, Y: ${y}, Value: ${props.puzzleSetup[i]}`);
    lights.value.push({
      pos_x: x * 2 - (dim - 1),
      pos_y: 1,
      pos_z: y * 2 - (dim - 1),
      isOn: props.puzzleSetup[i] === '1'
    });
  }

  console.log(lights.value);

  const toggleLight = (index: number) => {
    console.log(`Toggling light at index: ${index}`);

    // toggle clicked light
    lights.value[index].isOn = !lights.value[index].isOn;

    // toggle adjacent lights
    const dimNum = Math.sqrt(length);
    const adjacentIndices = [];

      //check to make sure left light is not on the previous row
      if (index - 1 % dimNum !== dimNum - 1) adjacentIndices.push(index - 1);
      //check to make sure right light is not on the next row
      if (index + 1 % dimNum !== 0) adjacentIndices.push(index + 1); 
      //check to make sure up light is in bounds
      if (index - dimNum >= 0) adjacentIndices.push(index - dimNum);
      //check to make sure down light is in bounds
      if (index + dimNum < length) adjacentIndices.push(index + dimNum);

      adjacentIndices.forEach((adjIndex) => {
        if (adjIndex >= 0 && adjIndex < length) {
          lights.value[adjIndex].isOn = !lights.value[adjIndex].isOn;
        }
      });
      console.log(lights.value);
  }

</script>

<template>
    <ColorSwitchingLight
        v-for="(i, index) in lights"
        :key="i.id"
        :pos_x="i.pos_x" 
        :pos_y="i.pos_y" 
        :pos_z="i.pos_z" 
        :colorOn="props.colorOn" 
        :colorOff="props.colorOff" 
        :isOn="i.isOn"
        @toggle="toggleLight(index)"
    />

    <EffectComposer>
      <FffectBloom
        :intensity="1.6"
        :luminanceThreshold="0.2"
        :luminanceSmoothing="0.9"
        :radius="0.4"
      />
    </EffectComposer>
</template>
