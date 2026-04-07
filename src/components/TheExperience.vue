<script setup lang="ts">
import type { TresObject } from '@tresjs/core'
import { useLoop } from '@tresjs/core'
import { shallowRef } from 'vue'
import ColorSwitchPuzzle from './ColorSwitchPuzzle.vue';

const { onBeforeRender } = useLoop()

const boxRef = shallowRef<TresObject | null>(null)

onBeforeRender(({ elapsed }) => {
  if (boxRef.value) {
    boxRef.value.rotation.y = elapsed
    boxRef.value.rotation.z = elapsed
  }
})

// ordered: On, Off
const colorPairs: [string, string][] = [
  ["#02c926", "#f90202"], //green, red 
  ["#1e32ea", "#ea9f1e"], //blue, orange
  ["#eef224", "#921ace"], //yellow, purple
]

const randomIndex: number = Math.floor(Math.random() * colorPairs.length)

const colorOn: string = colorPairs[randomIndex][0];
const colorOff: string = colorPairs[randomIndex][1];
</script>

<template>
  <TresPerspectiveCamera :position="[0, 10, 4]" :look-at="[0, 0, 0]" />
  <ColorSwitchPuzzle  puzzleSetup="000010000" :colorOn="colorOn" :colorOff="colorOff"/>
  <!-- <ColorSwitchPuzzle puzzleSetup="0000000000000000" :colorOn="colorOn" :colorOff="colorOff"/> -->
  <!-- <ColorSwitchPuzzle puzzleSetup="0000000000000000000000000" :colorOn="colorOn" :colorOff="colorOff"/> -->
  <TresAxesHelper />
  <TresGridHelper :args="[10, 10]" />
</template>
