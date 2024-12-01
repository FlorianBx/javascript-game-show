<script setup lang="ts">
import { ref, computed } from 'vue'
import { onSlideEnter, onSlideLeave } from '@slidev/client'

const timeLeft = ref(300) // 5 minutes
const isRunning = ref(true)
let intervalId

const timeString = computed(() => {
  const minutes = Math.floor(timeLeft.value / 60)
  const seconds = timeLeft.value % 60
  return `${minutes}:${seconds.toString().padStart(2, '0')}`
})

const timerColor = computed(() => {
  if (timeLeft.value < 60) return 'text-red-500'
  if (timeLeft.value < 120) return 'text-yellow-500'
  return 'text-green-500'
})

function toggleTimer() {
  isRunning.value = !isRunning.value
}

function resetTimer() {
  timeLeft.value = 300
  isRunning.value = true
}

onSlideEnter(() => {
  resetTimer()
  intervalId = setInterval(() => {
    if (isRunning.value && timeLeft.value > 0) {
      timeLeft.value--
    }
  }, 1000)
})

onSlideLeave(() => {
  clearInterval(intervalId)
})
</script>

<template>
  <div
    class="fixed top-4 right-4 flex items-center gap-2 bg-gray-800/75 backdrop-blur p-4 rounded-lg shadow-lg"
  >
    <div :class="[timerColor, 'i-lucide-alarm-clock text-2xl']" />
    <span :class="[timerColor, 'font-mono text-2xl font-bold']">{{ timeString }}</span>
    <button
      @click="toggleTimer"
      class="ml-2 px-2 py-1 bg-gray-700 rounded hover:bg-gray-600 transition-colors"
    >
      {{ isRunning ? '⏸' : '▶️' }}
    </button>
    <button
      @click="resetTimer"
      class="ml-2 px-2 py-1 bg-gray-700 rounded hover:bg-gray-600 transition-colors"
    >
      🔄
    </button>
  </div>
</template>
