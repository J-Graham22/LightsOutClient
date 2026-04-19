<template>
    <div
      class="w-3 h-3 rounded-full"
      :style="{ backgroundColor: color }"
    ></div>
  </template>
  
  <script setup>
  defineProps({
    color: {
      type: String,
      default: '#ff0000'
    }
  })
  </script>
  