<script setup>
import { computed, ref } from 'vue'
import QuizCard from './QuizCard.vue'

const quizzes = [
  {
    title: 'Общие знания',
    description: 'Вопросы по истории, географии, науке и культуре.',
    questions: [
      { text: 'Какая планета ближе всего к Солнцу?', answers: ['Земля', 'Меркурий', 'Марс'], correct: 1 },
      { text: 'Какой город является столицей Франции?', answers: ['Париж', 'Рим', 'Берлин'], correct: 0 },
      { text: 'Сколько дней в високосном году?', answers: ['365', '366', '367'], correct: 1 }
    ]
  },
  {
    title: 'Кино',
    description: 'Вопросы о фильмах, актёрах и сериалах.',
    questions: [
      { text: 'Как зовут главного героя серии фильмов «Пираты Карибского моря»?', answers: ['Джек Воробей', 'Гарри Поттер', 'Тони Старк'], correct: 0 },
      { text: 'В каком фильме есть персонаж Дарт Вейдер?', answers: ['Аватар', 'Звёздные войны', 'Шрек'], correct: 1 },
      { text: 'Кто снял фильм «Титаник» 1997 года?', answers: ['Джеймс Кэмерон', 'Кристофер Нолан', 'Стивен Спилберг'], correct: 0 }
    ]
  },
  {
    title: 'Компьютерные игры',
    description: 'Вопросы о компьютерных и видеоиграх.',
    questions: [
      { text: 'В какой игре можно строить из блоков?', answers: ['Minecraft', 'Tetris', 'Pac-Man'], correct: 0 },
      { text: 'Как зовут водопроводчика из игр Nintendo?', answers: ['Соник', 'Марио', 'Линк'], correct: 1 },
      { text: 'Что нужно собирать в игре Pac-Man?', answers: ['Монеты', 'Точки', 'Звёзды'], correct: 1 }
    ]
  },
  {
    title: 'Музыка',
    description: 'Вопросы о музыке.',
    questions: [
      { text: 'Сколько струн у обычной гитары?', answers: ['Четыре', 'Шесть', 'Восемь'], correct: 1 },
      { text: 'На каком инструменте играют с помощью клавиш?', answers: ['Барабан', 'Скрипка', 'Пианино'], correct: 2 },
      { text: 'Как называется человек, который руководит оркестром?', answers: ['Дирижёр', 'Актёр', 'Художник'], correct: 0 }
    ]
  }
]

const selectedQuiz = ref(null)
const questionIndex = ref(0)
const selectedAnswer = ref(null)
const score = ref(0)
const finished = ref(false)

const currentQuestion = computed(() => selectedQuiz.value?.questions[questionIndex.value])

function startQuiz(title) {
  selectedQuiz.value = quizzes.find(quiz => quiz.title === title)
  questionIndex.value = 0
  selectedAnswer.value = null
  score.value = 0
  finished.value = false
}

function answer(index) {
  if (selectedAnswer.value !== null) return
  selectedAnswer.value = index
  if (index === currentQuestion.value.correct) score.value++
}

function nextQuestion() {
  if (questionIndex.value + 1 === selectedQuiz.value.questions.length) {
    finished.value = true
  } else {
    questionIndex.value++
    selectedAnswer.value = null
  }
}
</script>

<template>
  <main>
    <template v-if="!selectedQuiz">
      <h1>Доступные викторины</h1>
      <section class="cards">
        <QuizCard
          v-for="quiz in quizzes"
          :key="quiz.title"
          :title="quiz.title"
          :description="quiz.description"
          @start="startQuiz"
        />
      </section>
    </template>

    <section v-else class="quiz">
      <h1>{{ selectedQuiz.title }}</h1>
      <template v-if="!finished">
        <p>Вопрос {{ questionIndex + 1 }} из {{ selectedQuiz.questions.length }}</p>
        <h2>{{ currentQuestion.text }}</h2>
        <div class="answers">
          <button
            v-for="(item, index) in currentQuestion.answers"
            :key="item"
            type="button"
            :disabled="selectedAnswer !== null"
            :class="{
              correct: selectedAnswer !== null && index === currentQuestion.correct,
              wrong: selectedAnswer === index && index !== currentQuestion.correct
            }"
            @click="answer(index)"
          >{{ item }}</button>
        </div>
        <template v-if="selectedAnswer !== null">
          <p class="message">{{ selectedAnswer === currentQuestion.correct ? 'Правильно!' : 'Неправильно.' }}</p>
          <button type="button" @click="nextQuestion">
            {{ questionIndex + 1 === selectedQuiz.questions.length ? 'Показать результат' : 'Следующий вопрос' }}
          </button>
        </template>
      </template>
      <template v-else>
        <h2>Викторина завершена</h2>
        <p>Ваш результат: {{ score }} из {{ selectedQuiz.questions.length }}.</p>
        <button type="button" @click="startQuiz(selectedQuiz.title)">Пройти ещё раз</button>
      </template>
      <button class="back-button" type="button" @click="selectedQuiz = null">К списку викторин</button>
    </section>
  </main>
</template>
