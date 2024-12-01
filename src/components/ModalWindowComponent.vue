<script setup>
import { ref } from 'vue'

const isEditModalOpen = ref(false)
const editableRow = ref({})

// Открытие модального окна для редактирования
const openEditModal = (row) => {
  editableRow.value = { ...row } // Копируем данные строки
  isEditModalOpen.value = true
}

// Закрытие модального окна
const closeEditModal = () => {
  isEditModalOpen.value = false
}

// Сохранение изменений
const saveEdit = () => {
  const rowIndex = rows.value.findIndex(
    (row) => row.id_user === editableRow.value.id_user,
  )
  if (rowIndex !== -1) {
    rows.value[rowIndex] = { ...editableRow.value }
  }
  closeEditModal()
}

</script>

<template>


</template>

<style scoped>

</style>
