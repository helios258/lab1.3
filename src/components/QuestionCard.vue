<script setup lang="ts">
import { computed } from 'vue'
import type { Question } from '../types'

const props = defineProps<{
  question: Question
  questionNumber: number
  totalQuestions: number
  modelValue: number | undefined
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: number): void
  (e: 'next'): void
}>()

const selectedOption = computed({
  get: () => props.modelValue,
  set: (val) => {
    if (val !== undefined) {
      emit('update:modelValue', val)
    }
  }
})
</script>

<template>
  <div class="question-card">
    <div class="header-info">
      <span class="badge">Запитання {{ questionNumber }} з {{ totalQuestions }}</span>
    </div>

    <h3 class="question-text">{{ question.text }}</h3>

    <div class="options-list">
      <label 
        v-for="(option, index) in question.options" 
        :key="index"
        class="option-label"
        :class="{ 'selected': selectedOption === index }"
      >
        <input 
          type="radio" 
          :name="`question-${question.id}`" 
          :value="index"
          v-model="selectedOption"
        />
        <span class="option-text">{{ option }}</span>
      </label>
    </div>

    <button 
      class="next-btn" 
      :disabled="selectedOption === undefined"
      @click="$emit('next')"
    >
      {{ questionNumber === totalQuestions ? 'Завершити тест' : 'Наступне запитання' }}
    </button>
  </div>
</template>

<style scoped>
.question-card {
  background: #ffffff;
  padding: 2rem;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  max-width: 600px;
  width: 100%;
  margin: 0 auto;
}

.badge {
  font-size: 0.85rem;
  color: #64748b;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.question-text {
  font-size: 1.25rem;
  color: #1e293b;
  margin: 1rem 0 1.5rem 0;
  line-height: 1.4;
}

.options-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  margin-bottom: 2rem;
}

.option-label {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1rem 1.25rem;
  border: 2px solid #e2e8f0;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
  background-color: #f8fafc;
}

.option-label:hover {
  border-color: #cbd5e1;
  background-color: #f1f5f9;
}

.option-label.selected {
  border-color: #6366f1;
  background-color: #eef2ff;
}

.option-label input[type="radio"] {
  accent-color: #6366f1;
  width: 1.2rem;
  height: 1.2rem;
}

.option-text {
  font-size: 1rem;
  color: #334155;
}

.next-btn {
  width: 100%;
  background-color: #6366f1;
  color: white;
  border: none;
  padding: 0.875rem;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.next-btn:hover:not(:disabled) {
  background-color: #4f46e5;
}

.next-btn:disabled {
  background-color: #cbd5e1;
  cursor: not-allowed;
}
</style>