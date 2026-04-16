<script lang="ts">
export type PuzzleState = {
  /** Per-cell: true = color "on", false = color "off" */
  onStates: boolean[]
  /** Same grid as `puzzleSetup`: `'1'` on, `'0'` off */
  stringRep: string
}
</script>

<script setup lang="ts">
// No imports needed! TresJS components are available globally
  import { ref, watch } from 'vue';
  import ColorSwitchingLight from './ColorSwitchingLight.vue';

  const emit = defineEmits<{
    puzzleStateChange: [state: PuzzleState]
  }>()

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

  console.log(props);
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

  //const lights = ref([]);
  const lights = ref<
    {
      pos_x: number
      pos_y: number
      pos_z: number
      isOn: boolean
    }[]
  >([])

  const rebuildLights = (puzzleSetup: string) => {
    const length = puzzleSetup.length
    const dim = Math.sqrt(length)
    const newLights: {
      pos_x: number
      pos_y: number
      pos_z: number
      isOn: boolean
    }[] = []

    for (let i = 0; i < length; i++) {
      const x = i % dim
      const y = Math.floor(i / dim)
      console.log(`Index: ${i}, X: ${x}, Y: ${y}, Value: ${puzzleSetup[i]}`)
      newLights.push({
        pos_x: x * 2 - (dim - 1),
        pos_y: 1,
        pos_z: y * 2 - (dim - 1),
        isOn: puzzleSetup[i] === '1',
      })
    }

    lights.value = newLights
    console.log(lights.value)
    notifyState()
  }

  const getCurrentStringRep = () => {
    let strRep = "";

    for(let i = 0; i < lights.value.length; i++) {
      strRep += lights.value[i].isOn ? "1" : "0";
    }
    
    return strRep;
  }

  const getPuzzleState = (): PuzzleState => ({
    onStates: lights.value.map((l) => l.isOn),
    stringRep: getCurrentStringRep(),
  })

  const notifyState = () => {
    emit('puzzleStateChange', getPuzzleState())
  }

  watch(
    () => props.puzzleSetup,
    (nextPuzzleSetup) => {
      rebuildLights(nextPuzzleSetup)
    },
    { immediate: true }
  )

  const toggleLight = (index: number) => {
    console.log(`Toggling light at index: ${index}`);

    // toggle clicked light
    lights.value[index].isOn = !lights.value[index].isOn;

    // toggle adjacent lights
    const length = lights.value.length
    const dimNum = Math.sqrt(length);
    const adjacentIndices = [];

    const left = index - 1
    const right = index + 1
    const up = index - dimNum
    const down = index + dimNum

      //check to make sure left light is not on the previous row
      if (left % dimNum !== dimNum - 1) adjacentIndices.push(left);
      //check to make sure right light is not on the next row
      if (right % dimNum !== 0) adjacentIndices.push(right); 
      //check to make sure up light is in bounds
      if (up >= 0) adjacentIndices.push(up);
      //check to make sure down light is in bounds
      if (down < length) adjacentIndices.push(down);

      adjacentIndices.forEach((adjIndex) => {
        if (adjIndex >= 0 && adjIndex < length) {
          lights.value[adjIndex].isOn = !lights.value[adjIndex].isOn;
        }
      });

    notifyState()
  }

</script>

<template>
    <!--
    <TresMesh
      :position="[0, 0, 0]"
    >
      <TresBoxGeometry :args="[1, 1, 1]" />
      <TresMeshStandardMaterial/>
    </TresMesh>
  -->
    <ColorSwitchingLight
        v-for="(i, index) in lights"
        :key="index"
        :pos_x="i.pos_x" 
        :pos_y="i.pos_y" 
        :pos_z="i.pos_z" 
        :colorOn="props.colorOn" 
        :colorOff="props.colorOff" 
        :isOn="i.isOn"
        @toggle="toggleLight(index)"
    />
</template>
