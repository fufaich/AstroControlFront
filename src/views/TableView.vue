<script setup>
  import { useRoute } from 'vue-router';
  import TableComponent from '@/components/TableComponent.vue'
  const route = useRoute();
  const tableName = route.params.tableName
  // console.log(tableName);

</script>

<template>
<div class="flex-col">
  <TableComponent :table_name="tableName"></TableComponent>
</div>
</template>

<style scoped>

</style>
