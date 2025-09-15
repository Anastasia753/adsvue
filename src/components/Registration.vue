<script setup>
import {ref} from "vue";
import router from "@/router/index.js";

const data = ref({
  email: '',
  password: '',
})
const errors = ref({});

const register = async () => {
  try {
    errors.value = {
      email: [],
      password: [],
    }
    const userData = {
      email: data.value.email,
      password: data.value.password,
    }

    const response = await fetch('http://127.0.0.1:8000/api/register', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(userData),
    })

    const result = await response.json()

    if (response.ok) {
      router.push('/login')
    } else {
      errors.value = result.errors
    }
  } catch (error) {
    console.log('Ошибка при регисрации: ', error)
  }
}
</script>

<template>
<div class="container">
  <form @submit.prevent="register">
    <h2>Регистрация</h2>
    <div>
      <label for="email">
        <input type="email" name="email" v-model="data.email" :class="{'is-invalid': errors.email}">
      </label>

      <div v-if="errors.email">
        <span>{{ errors.email[0] }}</span>
      </div>
    </div>

    <div>
      <label for="password">
        <input type="password" name="password" v-model="data.password" :class="{'is-invalid': errors.password}">
      </label>

      <div v-if="errors.password">
        <span>{{ errors.password[0] }}</span>
      </div>
    </div>

    <button type="submit">Зарегистрироваться</button>
  </form>
</div>
</template>

<style scoped>

</style>
