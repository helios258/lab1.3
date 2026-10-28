<script setup lang="ts">
import { ref, computed } from 'vue'
import QuestionCard from './components/QuestionCard.vue'
import ResultScreen from './components/ResultScreen.vue'
import type { Question } from './types'

const questions: Question[] = [
  {
    id: 1,
    text: "Що таке Vue.js?",
    options: [
      "Система керування базами даних",
      "Прогресивний JavaScript фреймворк",
      "Модуль стилів CSS нового покоління",
      "Операційна система для вебсерверів"
    ],
    correctIndex: 1
  },
  {
    id: 2,
    text: "Яка директива у Vue використовується для двостороннього зв'язування даних?",
    options: [
      "v-bind",
      "v-model",
      "v-for",
      "v-if"
    ],
    correctIndex: 1
  },
  {
    id: 3,
    text: "Який синтаксис найчастіше використовується для реактивних змінних у Vue 3 Composition API?",
    options: [
      "ref() або reactive()",
      "state()",
      "var()",
      "const() async"
    ],
    correctIndex: 0
  },
  {
    id: 4,
    text: "Яка роль пропантів (Props) у компонентній архітектурі Vue?",
    options: [
      "Для передачі даних від дочірнього компонента до батьківського",
      "Для стилізації елементів всередині компонента",
      "Для передачі вхідних даних від батьківського компонента до дочірнього",
      "Для збереження даних у localStorage"
    ],
    correctIndex: 2
  },
  {
    id: 5,
    text: "Що робить утиліта Vite у сучасних Vue-проєктах?",
    options: [
      "Слугує бібліотекою для анімацій",
      "Забезпечує надзвичайно швидке збирання проєкту та запуск серверу розробки",
      "Керує маршрутизацією сторінок (Router)",
      "Тестує код на наявність помилок TypeScript"
    ],
    correctIndex: 1
  }
]

const currentIndex = ref<number>(0)
const userAnswers = ref<Record<number, number>>({})
const isFinished = ref<boolean>(false)

const currentQuestion = computed(() => questions[currentIndex.value])

const handleNext = () => {
  if (currentIndex.value < questions.length - 1) {
    currentIndex.value++
  } else {
    isFinished.value = true
  }
}

const score = computed(() => {
  let correctCount = 0
  questions.forEach((q, index) => {
    if (userAnswers.value[index] === q.correctIndex) {
      correctCount++
    }
  })
  return correctCount
})

const restartQuiz = () => {
  currentIndex.value = 0
  userAnswers.value = {}
  isFinished.value = false
}
</script>

<template>
  <main class="quiz-container">
    <header class="app-header">
      <h1>University Quiz App</h1>
      <p>Варіант 1: Інтерактивний тест на Vue 3 та TypeScript</p>
    </header>

    <QuestionCard 
      v-if="!isFinished"
      :question="currentQuestion"
      :questionNumber="currentIndex + 1"
      :totalQuestions="questions.length"
      v-model="userAnswers[currentIndex]"
      @next="handleNext"
    />

    <ResultScreen 
      v-else
      :score="score"
      :total="questions.length"
      @restart="restartQuiz"
    />
  </main>
</template>

<style>
body {
  font-family: 'Inter', sans-serif;
  background-color: #f1f5f9;
  margin: 0;
  padding: 0;
}

.quiz-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 2rem 1rem;
}

.app-header {
  text-align: center;
  margin-bottom: 2rem;
}

.app-header h1 {
  color: #0f172a;
  font-size: 2rem;
  margin-bottom: 0.5rem;
}

.app-header p {
  color: #64748b;
  font-size: 1rem;
}
</style>