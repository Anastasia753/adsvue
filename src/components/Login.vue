<script setup>
import {ref} from "vue";
import router from "@/router/index.js";

const email = ref('')
const password = ref('')
const errors = ref({})

const login = async () => {
  try {
    errors.value = {
      email: [],
      password: [],
    }

    const response = await fetch('https://adsapi/api/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        email: email.value,
        password: password.value,
      })
    })

    const result = await response.json()

    if (response.ok) {
      router.push('/')
    } else {
      errors.value = result.errors
    }
  } catch (error) {
    console.log("Ошибка при входе: ", error)
    console.log(error)
  }
}
</script>

<template>
  <div class="container">
    <form @submit.prevent="login">
      <h2>Авторизация</h2>
      <div>
        <label for="email">
          <input type="text" name="email" v-model="email" :class="{'is-invalid': errors.email}">
        </label>

        <div v-if="errors.email">
          <span>{{ errors.email[0] }}</span>
        </div>
      </div>

      <div>
        <label for="password">
          <input type="text" name="password" v-model="password" :class="{'is-invalid': errors.password}">
        </label>

        <div v-if="errors.password">
          <span>{{ errors.password[0] }}</span>
        </div>
      </div>

      <button type="submit">Войти</button>
    </form>
  </div>
</template>

<style scoped>

</style>
