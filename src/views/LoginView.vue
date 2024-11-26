<template>
  <div class="h-screen flex flex-col items-center justify-start bg-gray-100">
    <div class="h-12"></div>
    <div class="w-full max-w-md bg-white p-8 rounded-lg shadow-md">
      <h2 class="text-2xl font-bold text-gray-800 text-center mb-6">Вход</h2>
      <form @submit.prevent="handleLogin" class="space-y-4">
        <div>
          <label for="username" class="block text-sm font-medium text-gray-700">
            Логин:
          </label>
          <input
            id="username"
            type="username"
            v-model="username"
            required
            placeholder="Введите логин"
            class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
          />
        </div>
        <div>
          <label for="password" class="block text-sm font-medium text-gray-700">
            Пароль:
          </label>
          <input
            id="password"
            type="password"
            v-model="password"
            required
            placeholder="Введите пароль"
            class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
          />
        </div>
        <div>
          <button
            type="submit"
            class="w-full bg-indigo-600 text-white py-2 px-4 rounded-md shadow hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
          >
            Войти
          </button>
        </div>
      </form>
      <p v-if="error" class="mt-4 text-sm text-red-600 text-center">{{ error }}</p>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import axios from "axios";
import router from '@/router/index.js'

export default {
  setup() {
    // Реактивные данные
    const username = ref("");
    const password = ref("");
    const error = ref("");

    // Метод для обработки авторизации
    const handleLogin = async () => {
      error.value = ""; // Сброс ошибки
      try {
        // Отправка запроса на сервер
        const response = await axios.post("http://127.0.0.1:5000/login/", {
          username: username.value,
          password: password.value,
        });

        // Сохранение токена в localStorage
        const token = response.data.access_token;
        const role = response.data.role;
        localStorage.setItem("username", username.value);
        localStorage.setItem("role", role);
        localStorage.setItem("jwt", token);
        console.log(token);
        await router.push("/");

        // eslint-disable-next-line no-unused-vars
      } catch (err) {
        error.value = "Неправильный email или пароль.";
      }
    };

    // Возвращаем данные и методы для использования в шаблоне
    return {
      username,
      password,
      error,
      handleLogin,
    };
  },
};
</script>

<style>
/* Никаких дополнительных стилей не нужно, используем Tailwind CSS */
</style>
