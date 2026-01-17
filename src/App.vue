<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import TheExperience from './components/TheExperience.vue'
import FirstExperience from './components/FirstExperience.vue'
</script>

<template>
  <TresCanvas
    clear-color="#82DBC5"
    window-size
  >
    <FirstExperience/>
  </TresCanvas>
</template>
