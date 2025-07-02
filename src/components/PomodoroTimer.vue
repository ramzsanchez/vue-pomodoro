<template>
  <div class="flex flex-col items-center p-6 bg-white rounded-2xl shadow-xl w-80">
    <div class="relative w-48 h-48 mb-4">
      <svg class="absolute inset-0 w-full h-full" viewBox="0 0 100 100">
        <circle cx="50" cy="50" r="45" stroke="#e5e7eb" stroke-width="10" fill="none" />
        <circle
          cx="50"
          cy="50"
          r="45"
          stroke="#3b82f6"
          stroke-width="10"
          fill="none"
          stroke-linecap="round"
          :stroke-dasharray="circumference"
          :stroke-dashoffset="progress"
          class="transition-all duration-1000 ease-linear"
        />
      </svg>
      <div class="absolute inset-0 flex items-center justify-center text-3xl font-bold text-gray-800">
        {{ minutes }}:{{ seconds }}
      </div>
    </div>

    <div class="flex gap-4">
      <button @click="toggle" class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition">
        {{ isRunning ? 'Pause' : 'Start' }}
      </button>
      <button @click="reset" class="px-4 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700 transition">
        Reset
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onUnmounted } from 'vue';

const initialMinutes = 25;
const totalSeconds = initialMinutes * 60;

const timeLeft = ref(totalSeconds);
const isRunning = ref(false);
let timer = null;

const minutes = computed(() => String(Math.floor(timeLeft.value / 60)).padStart(2, '0'));
const seconds = computed(() => String(timeLeft.value % 60).padStart(2, '0'));

const circumference = 2 * Math.PI * 45;
const progress = computed(() => ((totalSeconds - timeLeft.value) / totalSeconds) * circumference);

function toggle() {
  isRunning.value = !isRunning.value;
  if (isRunning.value) {
    timer = setInterval(() => {
      if (timeLeft.value > 0) {
        timeLeft.value--;
      } else {
        clearInterval(timer);
        isRunning.value = false;
      }
    }, 1000);
  } else {
    clearInterval(timer);
  }
}

function reset() {
  clearInterval(timer);
  timeLeft.value = totalSeconds;
  isRunning.value = false;
}

onUnmounted(() => clearInterval(timer));
</script>
