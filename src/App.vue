<script setup>
import { RouterView } from 'vue-router'
import HeaderComponent from '@/components/HeaderComponent.vue'
</script>

<template>
  <div class="bg-gray-300 w-4/5 m-auto flex flex-col  justify-between shadow-xl mt-14 rounded-xl">
    <HeaderComponent />
    <RouterView />
  </div>
</template>
