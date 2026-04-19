<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import { onMounted, ref } from 'vue'
import TheExperience from './components/TheExperience.vue'
import WhiteButton from './components/WhiteButton.vue';
import type { PuzzleState } from './components/ColorSwitchPuzzle.vue'
import axios from 'axios';
import ColorCircle from './components/ColorCircle.vue';

const api = axios.create({
  baseURL: "http://127.0.0.1:8000"
})

const generatePuzzleInput = async (size: number) => {
  try {
    const response = await api.get(`/puzzle/solvable/${size}`);
    return response.data;
  } catch (err) {
    console.error('Failed to fetch puzzle input:', err);
    return null;
  }
}

const solveBruteForce = async () => {
  try {
    const numOfSolutions = 2 ** selectedPuzzleSize.value;
    statusText.value = `Solving via brute force! Evaluating ${numOfSolutions} possible solutions...`;
    const start = new Date(Date.now()).getTime();

    const response = await api.get(`/solution/brute_force/${puzzleState.value?.stringRep}`);
    const end = new Date(Date.now()).getTime();
    let timeTaken = end - start;
    updateWireframInput(response.data[0]);
    statusText.value = `Finished solving via brute force in ${timeTaken / 1000} seconds! Found ${response.data.length} solutions. Using the first one to solve!`;
    return response.data;
  } catch (err) {
    console.error('Failed to solve via brute force:', err);
    statusText.value = "Found some error while trying to solve via brute force...";
    return null;
  }
}

const solveMathematically = async () => {
  try {
    statusText.value = "Solving via matrix multiplication!";
    const response = await api.get(`/solution/matrix_multiplication/${puzzleState.value?.stringRep}`);
    let rep: string = '';
    for (let i = 0; i < response.data.length; i++) {
      rep += response.data[i].toString();
    }
    updateWireframInput(rep);
    statusText.value = "Found a solution! Click the lights highlighted with wireframe to solve";
    return response.data;
  } catch (err) {
    console.error('Failed to solve via matrix multiplication:', err);
    statusText.value = "This puzzle isn't able to be solved via matrix multiplication";
    return null;
  }
}

const puzzleInput = ref('')
const wireframeInput = ref('')
const onColor = ref('')
const offColor = ref('')
const loading = ref(true)
const selectedPuzzleSize = ref(9)
const statusText = ref('')

/** Latest grid from `ColorSwitchPuzzle`: each index is on/off; `stringRep` matches API format */
const puzzleState = ref<PuzzleState | null>(null)

const onPuzzleStateChange = (state: PuzzleState) => {
  puzzleState.value = state;
  updateWireframInput(puzzleState.value.wireframeStringRep)
  updatePuzzleInput(puzzleState.value.stringRep)
  if ([...puzzleState.value.stringRep].every(c => c == '1')) {
    statusText.value = "Solved!"
  }
  // console.log(puzzleState);
}

const updatePuzzleInput = (data: unknown) => {
  // generatePuzzleInput already returns axios response.data (the response body)
  if (typeof data === 'string') {
    puzzleInput.value = data
  } else if (data && typeof data === 'object' && 'data' in data) {
    puzzleInput.value = String((data as { data: string }).data)
  } else {
    puzzleInput.value = ''
  }
}

const updateWireframInput = (input: string | null) => {
  if (input == null) {
    var empty = ''
    for(let i = 0; i < puzzleInput.value.length; i++) {
      empty += '0';
    }
    wireframeInput.value = empty;
  } else {
    wireframeInput.value = input; 
  }
}

// ordered: On, Off
const colorPairs: [string, string][] = [
  ["#02c926", "#f90202"], //green, red 
  ["#1e32ea", "#ea9f1e"], //blue, orange
  ["#eef224", "#921ace"], //yellow, purple
]

const updateColors = () => {
  const randomIndex: number = Math.floor(Math.random() * colorPairs.length)

  onColor.value = colorPairs[randomIndex][0];
  offColor.value = colorPairs[randomIndex][1];
}

const generateNewPuzzle = async () => {
  const data = await generatePuzzleInput(selectedPuzzleSize.value)
  updatePuzzleInput(data)
  updateColors()
  updateWireframInput(null)
  statusText.value = '';
}

onMounted(async () => {
  const data = await generatePuzzleInput(selectedPuzzleSize.value)
  updatePuzzleInput(data)
  updateColors()
  updateWireframInput(null)
  loading.value = false
})
</script>

<template>
  <div class="relative w-screen h-screen">
    <h1 class="absolute top-4 left-1/2 z-10 -translate-x-1/2 text-center text-white">Lights Out!</h1>
    <div class="absolute top-10 left-1/2 z-10 -translate-x-1/2 text-center text-white">
      <div class="flex items-center gap-2">
        <h1>Target Color:</h1>
        <ColorCircle :color="onColor" />
      </div>
    </div>
    <h2 class="absolute top-18 left-1/2 z-10 -translate-x-1/2 text-center text-white">{{ statusText }}</h2>
    <div class="absolute bottom-4 left-0 right-0 z-10 flex justify-center gap-4">
      <WhiteButton label="Solve Mathematically" :onClick="solveMathematically"/>
      <WhiteButton label="Solve Brute Force" :onClick="solveBruteForce"/>
    </div>
    <div class="absolute top-1/2 left-4 z-10 flex -translate-y-1/2 flex-col items-start gap-3">
      <WhiteButton label="Generate new puzzle" :onClick="generateNewPuzzle"/>
      <div class="flex flex-col gap-2 text-white">
        <label class="flex items-center gap-2">
          <input v-model="selectedPuzzleSize" type="radio" :value="9" name="puzzle-size"/>
          <span>3 x 3</span>
        </label>
        <label class="flex items-center gap-2">
          <input v-model="selectedPuzzleSize" type="radio" :value="16" name="puzzle-size"/>
          <span>4 x 4</span>
        </label>
        <label class="flex items-center gap-2">
          <input v-model="selectedPuzzleSize" type="radio" :value="25" name="puzzle-size"/>
          <span>5 x 5</span>
        </label>
      </div>
    </div>
    <TresCanvas
      :physically-correct-lights="true"
      window-size
    >
      <div v-if="loading">Loading...</div>
      <TheExperience
        v-else
        :puzzle-input="puzzleInput"
        :wireframes-input="wireframeInput"
        :colorOn="onColor"
        :colorOff="offColor"
        @puzzle-state-change="onPuzzleStateChange"
      />
    </TresCanvas>
  </div>
</template>
