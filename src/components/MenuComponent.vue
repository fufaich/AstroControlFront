<script setup>
import ButtonComponent from '@/components/ButtonComponent.vue'
import { onMounted, ref } from 'vue'

const isAdmin = ref(false);

const checkRole = async () => {
  const role = localStorage.getItem('role');
  isAdmin.value = (role === "admin");

}




onMounted(() => {
  checkRole();
});
</script>

<template>
  <div class="flex flex-col items-center h-screen gap-4">
    <ButtonComponent label="Задачи" class="space-y-4 mt-4" path="/test"/>

    <ButtonComponent v-if="isAdmin" label="Пользователи" class="space-y-4" path="/test"/>
    <ButtonComponent v-if="isAdmin" label="Выполненные задачи" class="space-y-4" path="/test"/>

    <ButtonComponent label="Эксперименты" class="space-y-4" path="/test"/>
    <ButtonComponent label="Миссии" class="space-y-4" path="/test"/>
    <ButtonComponent label="Ресурсы" class="space-y-4" path="/test"/>
    <ButtonComponent label="Отчёты" class="space-y-4" path="/test"/>

  </div>
</template>


