<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import { onMounted, ref } from 'vue'
import TheExperience from './components/TheExperience.vue'
import WhiteButton from './components/WhiteButton.vue';
import axios from 'axios';

const api = axios.create({
  baseURL: "http://127.0.0.1:8000"
})

const generatePuzzleInput = async (size: number) => {
  try {
    const response = await api.get(`/puzzle/solvable/${size}`);
    console.log(response);
    return response.data;
  } catch (err) {
    console.error('Failed to fetch puzzle input:', err);
    return null;
  }
}

const solveBruteForce = () => {

}

const solveMathematically = () => {

}

const puzzleInput = ref('')
const loading = ref(true)

onMounted(async () => {
  const data = await generatePuzzleInput(25)
  // generatePuzzleInput already returns axios response.data (the response body)
  if (typeof data === 'string') {
    puzzleInput.value = data
  } else if (data && typeof data === 'object' && 'data' in data) {
    puzzleInput.value = String((data as { data: string }).data)
  } else {
    puzzleInput.value = ''
  }
  loading.value = false
})
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
      <div v-if="loading">Loading...</div>
      <TheExperience v-else :puzzle-input="puzzleInput"/>
    </TresCanvas>
  </div>
</template>
