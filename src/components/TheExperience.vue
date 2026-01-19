<script setup lang="ts">
import type { TresObject } from '@tresjs/core'
import { useLoop } from '@tresjs/core'
import { shallowRef } from 'vue'
import ColorSwitchingLight from './ColorSwitchingLight.vue';
import ColorSwitchPuzzle from './ColorSwitchPuzzle.vue';


const { onBeforeRender } = useLoop()

const boxRef = shallowRef<TresObject | null>(null)

onBeforeRender(({ elapsed }) => {
  if (boxRef.value) {
    boxRef.value.rotation.y = elapsed
    boxRef.value.rotation.z = elapsed
  }
})
</script>

<template>
  <TresPerspectiveCamera :position="[0, 10, 4]" :look-at="[0, 0, 0]" />
  <!-- <TresDirectionalLight :position="[-4,8,5]" color="red" :intensity="2" /> -->
  <!--<ColorSwitchingLight :pos_x="1" :pos_y="1" :pos_z="1" colorOn="yellow" colorOff="yellow"/>-->
  <ColorSwitchPuzzle puzzleSetup="100010011" colorOn="yellow" colorOff="black"/>
  <TresAxesHelper />
  <TresGridHelper :args="[10, 10]" />
</template>
