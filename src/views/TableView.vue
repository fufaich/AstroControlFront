<script setup>
  import { useRoute } from 'vue-router';
  import axios from 'axios'
  import { onMounted} from 'vue'
  import TableComponent from '@/components/TableComponent.vue'
  const route = useRoute();
  const tableName = route.params.tableName
  // console.log(tableName);

</script>

<template>
<div class="flex-col">
  <h2 class="font-normal text-4xl">Таблица: {{tableName}}</h2>
  <TableComponent :table_name="tableName"></TableComponent>
</div>
</template>

<style scoped>

</style>
