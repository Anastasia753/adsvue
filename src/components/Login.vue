<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const email = ref('');
const password = ref('');
const errorMessage = ref({ email: '', password: '' });
const router = useRouter();
const apihost = 'http://127.0.0.1:8000/api';

const tryLogin = async () => {
  errorMessage.value = { email: '', password: '' };

  if (!email.value) {
    errorMessage.value.email = 'Пожалуйста, введите Email.';
  }

  if (!password.value) {
    errorMessage.value.password = 'Пожалуйста, введите пароль.';
  }

  // Если есть ошибки, не продолжаем
  if (errorMessage.value.email || errorMessage.value.password) {
    return;
  }

  try {
    let response = await fetch(apihost + '/login', {
      method: "POST",
      headers: {
        'Content-type': 'application/json',
      },
      body: JSON.stringify({
        email: email.value,
        password: password.value
      })
    });
    console.log('Отправляемые данные:', { email: email.value, password: password.value });

    const login = await response.json();
    console.log('Ответ от сервера:', login);
    if (response.ok) {
      localStorage.setItem('token', login.token);
      localStorage.setItem('email', login.email);

      router.push('/');
    } else {
      errorMessage.value.email = "Неправильный Email или пароль"; // Делаем так, чтобы ошибка отображалась под Email
      errorMessage.value.password = "Неправильный Email или пароль"; // Также можно сделать так, чтобы выводилась ошибка под паролем
    }
  } catch (error) {
    console.log('Ошибка при входе ', error);
    errorMessage.value.email = "Ошибка сервера, попробуйте позже.";
    errorMessage.value.password = "Ошибка сервера, попробуйте позже.";
  }
};
</script>

<template>
  <div class="container">
    <h2 class="text-center">Авторизация</h2>
    <form id="registrationForm" @submit.prevent="tryLogin">
      <div class="form-group">
        <label for="email" class="form-label">Email</label>
        <input
          type="text"
          class="form-control"
          :class="{'border-danger': errorMessage.email}"
          id="email"
          v-model="email"
        />
        <div v-if="errorMessage.email" class="text-danger mt-1">{{ errorMessage.email }}</div>
      </div>

      <div class="form-group">
        <label for="password" class="form-label">Пароль</label>
        <input
          v-model="password"
          type="password"
          class="form-control"
          :class="{'border-danger': errorMessage.password}"
          id="password"
        />
        <div v-if="errorMessage.password" class="text-danger mt-1">{{ errorMessage.password }}</div>
      </div>

      <button
        type="submit"
        class="btn btn-warning btn-block"
        style="background-color: #ff9900; border: none; margin-top: 10px"
      >
        Войти
      </button>
    </form>
  </div>
</template>

<style scoped>
.container {
  width: 30%;
  margin-top: 100px;
  border: 1px solid #ffcc00;
  padding: 30px;
  border-radius: 10px;
}

.form-label {
  font-weight: bold;
}

.btn-warning {
  transition: background-color 0.3s, transform 0.2s;
}

.btn-warning:hover {
  background-color: #ffcc00;
  transform: scale(1.05);
}
</style>
