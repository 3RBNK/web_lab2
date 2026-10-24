<script setup>
defineProps({
  title: String,
  description: String
})

const emit = defineEmits(['start'])
</script>

<template>
  <div class="card">
    <h2>{{ title }}</h2>
    <p>{{ description }}</p>
    <button type="button" @click="emit('start', title)">Начать</button>
  </div>
</template>
