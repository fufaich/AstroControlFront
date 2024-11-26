<template>
  <router-link :to="path" class="bg-blue-300 text-white py-2 px-4 rounded-xl font-semibold hover:bg-blue-500 transition duration-300 w-1/5">
    {{ label }}
  </router-link>
</template>

<script setup>
import { defineProps } from 'vue';
defineProps({
  label: {
    type: String,
    required: true,
  },
  path: {
    type: String,
    required: true,
  },
})
</script>

