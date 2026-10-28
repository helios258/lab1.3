<script setup lang="ts">
defineProps<{
  score: number
  total: number
}>()

defineEmits<{
  (e: 'restart'): void
}>()
</script>

<template>
  <div class="result-screen">
    <div class="result-icon">🏆</div>
    <h2>Тест завершено!</h2>
    <p class="score-text">
      Ваш результат: <span class="highlight">{{ score }} з {{ total }}</span> правильних відповідей
    </p>
    <button class="restart-btn" @click="$emit('restart')">
      Пройти тест знову
    </button>
  </div>
</template>

<style scoped>
.result-screen {
  background: #ffffff;
  padding: 2.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  text-align: center;
}

.result-icon {
  font-size: 3.5rem;
  margin-bottom: 1rem;
}

h2 {
  color: #1e293b;
  margin-bottom: 1rem;
}

.score-text {
  font-size: 1.15rem;
  color: #475569;
  margin-bottom: 2rem;
}

.highlight {
  font-weight: 700;
  color: #6366f1;
  font-size: 1.3rem;
}

.restart-btn {
  background-color: #6366f1;
  color: white;
  border: none;
  padding: 0.875rem 2rem;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.restart-btn:hover {
  background-color: #4f46e5;
}
</style>