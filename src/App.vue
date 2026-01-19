<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import TheExperience from './components/TheExperience.vue'
import FirstExperience from './components/FirstExperience.vue'
import ColorSwitchingLight from './components/ColorSwitchingLight.vue';

//    clear-color="#82DBC5"
</script>

<template>
  <TresCanvas
    :physically-correct-lights="true"
    window-size
  >
    <!--<FirstExperience/>-->
    <TheExperience/>
  </TresCanvas>
</template>
