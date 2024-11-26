<template>
  <div class="flex flex-col items-center justify-center p-4">
    <h2 class="text-2xl font-bold text-gray-800">Профиль пользователя</h2>
    <div class="mt-4 text-lg">
      <p><strong>Имя:</strong> {{ username }}</p>
      <p><strong>Роль:</strong> {{ role }}</p>
    </div>
    <button
      @click="logout"
      class="mt-6 bg-red-600 text-white py-2 px-4 rounded-full font-semibold hover:bg-red-700 transition duration-300"
    >
      Выйти
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// Реактивные переменные для имени пользователя и роли
const username = ref(localStorage.getItem('username') || 'Гость');
const role = ref(localStorage.getItem('role') || 'Не определена');

// Метод для выхода из аккаунта
const logout = () => {
  localStorage.removeItem('jwt');
  localStorage.removeItem('username');
  localStorage.removeItem('role');

  // Обновление переменных (опционально)
  username.value = 'Гость';
  role.value = 'Не определена';

  // Дополнительные действия, например, перенаправление
  // Например, если используете Vue Router:
  window.location.reload(); // Для перезагрузки страницы
};
</script>

<style scoped>
/* Дополнительные стили (если нужны) */
</style>
