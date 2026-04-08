<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import TheExperience from './components/TheExperience.vue'
import WhiteButton from './components/WhiteButton.vue';
import axios from 'axios';

const api = axios.create({
  baseURL: "http://127.0.0.1"
})

const generatePuzzleInput = async (size: number) => {
  try {
    const response = await api.get("/puzzle/solvable/${size}");
    return response.data;
  } catch (err) {
    return err;
  }
}

const solveBruteForce = () => {

}

const solveMathematically = () => {

}
</script>

<template>
  <div class="relative w-screen h-screen">
    <h1 class="absolute top-4 left-1/2 z-10 -translate-x-1/2 text-center text-white">Lights Out!</h1>
    <div class="absolute bottom-4 left-0 right-0 z-10 flex justify-center gap-4">
      <WhiteButton label="Solve Mathematically"/>
      <WhiteButton label="Solve Brute Force"/>
    </div>
    <TresCanvas
      :physically-correct-lights="true"
      window-size
    >
      <TheExperience :puzzle-input="puzzleInput"/>
    </TresCanvas>
  </div>
</template>
