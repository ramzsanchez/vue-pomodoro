<template>
  <div class="min-h-screen bg-gradient-to-br from-purple-100 to-blue-100 flex flex-col items-center justify-center">
    <h1 class="text-4xl font-bold text-blue-900 mb-6">Pomodoro Timer</h1> <!-- ✅ Visible title -->
    <PomodoroTimer />
  </div>
</template>

<script setup>
import PomodoroTimer from './components/PomodoroTimer.vue';
</script>
