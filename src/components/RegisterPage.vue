<script setup>
import { ref } from 'vue'

const username = ref('')
const email = ref('')
const password = ref('')
const passwordRepeat = ref('')
const message = ref('')

function submitForm() {
  if (password.value !== passwordRepeat.value) {
    message.value = 'Пароли не совпадают.'
    return
  }
  message.value = 'Форма заполнена. Регистрация в этой лабораторной не предусмотрена.'
}
</script>

<template>
  <main>
    <form @submit.prevent="submitForm">
      <h1>Регистрация</h1>
      <label for="register-username">Имя пользователя</label>
      <input id="register-username" v-model="username" type="text" placeholder="Введите имя" required>

      <label for="register-email">Электронная почта</label>
      <input id="register-email" v-model="email" type="email" placeholder="example@mail.ru" required>

      <label for="register-password">Пароль</label>
      <input id="register-password" v-model="password" type="password" placeholder="Введите пароль" required>

      <label for="password-repeat">Повторите пароль</label>
      <input id="password-repeat" v-model="passwordRepeat" type="password" placeholder="Повторите пароль" required>

      <button type="submit">Зарегистрироваться</button>
      <p v-if="message" class="message">{{ message }}</p>
      <p>Уже есть аккаунт? <a href="#/login">Войти</a></p>
    </form>
  </main>
</template>
