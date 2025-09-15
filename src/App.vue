<script setup>
import { RouterLink, RouterView } from 'vue-router'
import router from '@/router/index.js'

const email = localStorage.getItem('email');
console.log(email);
const token = localStorage.getItem('token');
console.log(token);
const logout = async () => {
  if (confirm("Вы уверены, что хотите выйти?")) {
    localStorage.removeItem('token');
    localStorage.removeItem('email');
    await fetch("http://127.0.0.1:8000/api/logout", { method: "POST" });
    router.push('/login')
  }
};
</script>

<template>
  <div class="container pt-5">
    <nav class="d-flex justify-content-between">
      <a href="/" class="text-decoration-none ">Объявления</a>
      <ul class="d-flex justify-content-between align-items-center gap-4 list-unstyled">
        <li><router-link v-if="!token" to="/register" class="text-decoration-none text-dark">Регистрация</router-link></li>
        <li><a v-if="!token" href="/login" class="text-decoration-none text-dark">Авторизация</a></li>
        <li><a v-if="token" href="" class="text-decoration-none text-dark">Список объявлений</a></li>
        <li><a v-if="token" href="" class="text-decoration-none text-dark">Список своих объявлений</a></li>
        <li><a v-if="token" href="" class="text-decoration-none text-dark">Создание объявления</a></li>
        <li><button v-if="token" type="button" @click="logout" class="text-decoration-none text-dark">Выйти</button></li>
      </ul>
    </nav>
  </div>

  <RouterView />
</template>

<style scoped>
</style>
