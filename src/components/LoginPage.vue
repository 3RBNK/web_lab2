<script setup>
import { ref } from 'vue'

const email = ref('')
const password = ref('')
const message = ref('')

function submitForm() {
  message.value = 'Форма заполнена.'
}
</script>

<template>
  <main>
    <form @submit.prevent="submitForm">
      <h1>Вход</h1>
      <label for="login-email">Электронная почта</label>
      <input id="login-email" v-model="email" type="email" placeholder="example@mail.ru" required>

      <label for="login-password">Пароль</label>
      <input id="login-password" v-model="password" type="password" placeholder="Введите пароль" required>

      <button type="submit">Войти</button>
      <p v-if="message" class="message">{{ message }}</p>
      <p>Нет аккаунта? <a href="#/register">Зарегистрироваться</a></p>
    </form>
  </main>
</template>
