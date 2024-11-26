<script setup>
import { onMounted } from 'vue';
import { useRouter } from 'vue-router'; // Импортируйте useRouter для навигации
import MenuComponent from '@/components/MenuComponent.vue';
import CardUser from '@/components/CardUser.vue';

const router = useRouter(); // Создайте экземпляр router

// Функция для проверки токена
const checkToken = () => {
  const token = localStorage.getItem('jwt'); // Получаем токен из localStorage

  // Проверка наличия токена и его истечения
  if (!token) {
    router.push('/login'); // Перенаправление на страницу логина, если токен отсутствует
  }

  // Здесь можно добавить дополнительную логику, чтобы проверить,
  // не истек ли токен (при наличии информации о времени жизни токена).
  // Например, если токен в формате JWT, можно проверить его срок действия:
  // const payload = JSON.parse(atob(token.split('.')[1]));
  // const isExpired = Date.now() >= payload.exp * 1000;
  // if (isExpired) {
  //   router.push('/login');
  // }
};

// Проверка токена при монтировании компонента
onMounted(() => {
  checkToken();
});
</script>

<template>
  <CardUser></CardUser>
  <MenuComponent/>
</template>

<style scoped>

</style>
