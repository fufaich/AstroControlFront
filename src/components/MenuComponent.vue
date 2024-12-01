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
    <ButtonComponent label="Журнал задач" class="space-y-4 mt-4" path="/table/task_journal"/>

    <ButtonComponent v-if="isAdmin" label="Пользователи" class="space-y-4" path="/table/users"/>
    <ButtonComponent v-if="isAdmin" label="Журнал выполненных задач" class="space-y-4" path="/table/completed_tasks"/>

    <ButtonComponent label="Журнал экспериментов" class="space-y-4" path="/table/experiment_journal"/>
    <ButtonComponent label="Миссии" class="space-y-4" path="/table/missions"/>
    <ButtonComponent label="Ресурсы" class="space-y-4" path="/table/resources"/>
    <ButtonComponent label="Работники" class="space-y-4" path="/table/employees"/>
    <ButtonComponent label="Отчёты" class="space-y-4" path="/table/reports_journal"/>


  </div>
</template>


