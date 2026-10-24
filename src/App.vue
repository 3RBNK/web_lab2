<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import SiteHeader from './components/SiteHeader.vue'
import HomePage from './components/HomePage.vue'
import QuizzesPage from './components/QuizzesPage.vue'
import LoginPage from './components/LoginPage.vue'
import RegisterPage from './components/RegisterPage.vue'

const page = ref('')

function updatePage() {
  page.value = window.location.hash.slice(1) || '/'
}

onMounted(() => {
  updatePage()
  window.addEventListener('hashchange', updatePage)
})

onUnmounted(() => window.removeEventListener('hashchange', updatePage))
</script>

<template>
  <SiteHeader />
  <HomePage v-if="page === '/'" />
  <QuizzesPage v-else-if="page === '/quizzes'" />
  <LoginPage v-else-if="page === '/login'" />
  <RegisterPage v-else-if="page === '/register'" />
  <main v-else>
    <h1>Страница не найдена</h1>
    <a href="#/">Вернуться на главную</a>
  </main>
</template>
