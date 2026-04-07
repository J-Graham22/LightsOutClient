<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import TheExperience from './components/TheExperience.vue'

//    clear-color="#82DBC5"
</script>

<template>
  <div class="relative w-screen h-screen">
    <h1 class="absolute top-4 left-4 z-10 text-white">Lights Out!</h1>
    <TresCanvas
      :physically-correct-lights="true"
      window-size
    >
      <TheExperience/>
    </TresCanvas>
  </div>
</template>
