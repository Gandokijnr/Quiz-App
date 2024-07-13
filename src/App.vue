<script setup>
import { ref, computed } from 'vue';

const questions = ref([
  {
    question: 'The Nigeria flags comprises of how many colors?',
    answers: 1,
    options: ['2', '3', '1', 'All of the above'],
    selected: null
  },
  {
    question: 'The first Nigerian president was who?',
    answers: 3,
    options: ['Benjamin Musa Yoweri', 'Adeyemi Abiola', 'Akandele Babatunde', 'None of the above'],
    selected: null
  },
  {
    question: 'Who is the richest man in Nigeria?',
    answers: 0,
    options: ['Aliko Dangote', 'Otedola', 'Benjamin Musa Yoweri', 'All of the above'],
    selected: null
  },
  {
    question: 'Who is the current president of Nigeria?',
    answers: 2,
    options: ['Akandele Babatunde', 'Babajide Sanwo Olu', 'Bola Ahmed Tinubu', 'None of the above'],
    selected: null
  }
]);

const currentQuestionIndex = ref(0);
const completed = ref(false);
const correctAnswers = computed(() => {
  return questions.value.filter(q => q.selected === q.answers).length;
});
const currentQuestion = computed(() => questions.value[currentQuestionIndex.value]);

const setAnswer = (optionIndex) => {
  currentQuestion.value.selected = optionIndex;
};

const nextQuestion = () => {
  if (currentQuestionIndex.value < questions.value.length - 1) {
    currentQuestionIndex.value++;
  } else {
    completed.value = true;
  }
};

const resetQuiz = () => {
  questions.value.forEach(q => q.selected = null);
  currentQuestionIndex.value = 0;
  completed.value = false;
};
</script>

<template>
<div class="container mx-auto h-96 mt-5">
  <div class="flex flex-col items-center justify-center h-full">
    <h1 class="text-3xl text-center p-3 w-full bg-white text-black rounded">Nigerian Quiz</h1>
    <p v-if="!completed" class="text-center p-3 w-full mt-3">Carefully select the correct answer:</p>
    <div v-if="!completed" class="flex flex-col gap-4 mt-5">
      <h2 class="text-lg text-center">{{ currentQuestion.question }}</h2>
      <div class="flex flex-col gap-2">
        <button
          class="bg-green-950 p-2 rounded"
          v-for="(option, index) in currentQuestion.options"
          :key="option"
          @click="setAnswer(index)"
          :class="{ 'border-2 border-white p-2 rounded': currentQuestion.selected === index }"
        >
          {{ option }}
        </button>
      </div>
      <button @click="nextQuestion" class="bg-white text-black py-2 px-4 rounded-md text-sm">
        Next Question
      </button>
    </div>
    <div v-else class="flex flex-col gap-4">
      <h2 class="text-lg text-center">Quiz Completed!</h2>
      <p class="text-lg text-center">Your score: {{ correctAnswers }}</p>
      <button @click="resetQuiz" class="bg-white text-black py-2 px-4 rounded-md text-sm">
        Reset Quiz
      </button>
      <p class="text-lg text-center">Total Questions: {{ questions.length }}</p>
      <p class="text-lg text-center">Correct Answers: {{ correctAnswers }}</p>
      <p class="text-lg text-center">Percentage: {{ (correctAnswers / questions.length) * 100 }}%</p>
      <p class="text-lg text-center">
        Follow me on twitter @ <a href="https://twitter.com/gand_tech" target="_blank">Gandoki Jr</a>
      </p>
    </div>
  </div>
</div>
</template>

<style>
body {
  padding: 2rem;
  background-color: black;
  color: white;
}
</style>
