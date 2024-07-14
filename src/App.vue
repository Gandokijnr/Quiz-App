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
    <div v-else class="flex flex-col gap-4 border-white border mt-5 shadow-white rounded p-8">
      <h2 class="text-lg text-center">Quiz Completed!</h2>
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-12 w-full">
        <path stroke-linecap="round" stroke-linejoin="round" d="M6.633 10.25c.806 0 1.533-.446 2.031-1.08a9.041 9.041 0 0 1 2.861-2.4c.723-.384 1.35-.956 1.653-1.715a4.498 4.498 0 0 0 .322-1.672V2.75a.75.75 0 0 1 .75-.75 2.25 2.25 0 0 1 2.25 2.25c0 1.152-.26 2.243-.723 3.218-.266.558.107 1.282.725 1.282m0 0h3.126c1.026 0 1.945.694 2.054 1.715.045.422.068.85.068 1.285a11.95 11.95 0 0 1-2.649 7.521c-.388.482-.987.729-1.605.729H13.48c-.483 0-.964-.078-1.423-.23l-3.114-1.04a4.501 4.501 0 0 0-1.423-.23H5.904m10.598-9.75H14.25M5.904 18.5c.083.205.173.405.27.602.197.4-.078.898-.523.898h-.908c-.889 0-1.713-.518-1.972-1.368a12 12 0 0 1-.521-3.507c0-1.553.295-3.036.831-4.398C3.387 9.953 4.167 9.5 5 9.5h1.053c.472 0 .745.556.5.96a8.958 8.958 0 0 0-1.302 4.665c0 1.194.232 2.333.654 3.375Z" />
      </svg>

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
      <hr class="mt-8 border-t-2 text-white" />
    <span>{{ currentQuestionIndex.question }}</span>
    <span>Question: {{ currentQuestionIndex + 1 }} of {{ questions.length }}</span>
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
