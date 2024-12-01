<script setup>

// const test = () => {console.log("TESTING")}
</script>

<template>
  <div class="bg-gray-200 border-b border-slate-200 py-8 px-10">
    <header>
      <ul class="flex items-center justify-start gap-5 p-2">
        <router-link to="/">
          <li class="flex items-center gap-3 text-gray-700 cursor-pointer hover:text-black">
            <img src="@/assets/astronaut-logo.svg" alt="Cart"  class="w-16 h-16"/>
            <p>Главная</p>
          </li>
        </router-link>
      </ul>
    </header>
  </div>

</template>
