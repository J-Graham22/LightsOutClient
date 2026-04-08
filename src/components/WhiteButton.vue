<template>
    <button
      type="button"
      class="cursor-pointer px-4 py-2 bg-white text-black font-semibold rounded-lg shadow transition hover:bg-gray-100 active:bg-gray-400"
      @click="onClick?.($event)"
    >
      {{ label }}
    </button>
  </template>
  
  <script setup>
  defineProps({
    label: {
      type: String,
      default: "Click Me",
    },
    onClick: {
      type: Function,
      default: undefined,
    },
  });
  </script>
  