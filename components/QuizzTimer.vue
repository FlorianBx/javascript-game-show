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
    class="fixed top-4 right-4 flex items-center gap-3 bg-zinc-900/50 backdrop-blur px-6 py-4 rounded-xl ring-1 ring-zinc-700/50"
  >
    <div :class="[timerColor, 'i-lucide-alarm-clock text-2xl']" />
    <span :class="[timerColor, 'font-mono text-2xl font-medium text-zinc-200']">
      {{ timeString }}
    </span>
    <div class="flex items-center gap-2 ml-2">
      <button
        @click="toggleTimer"
        class="px-3 py-2 bg-zinc-800/50 rounded-lg hover:bg-zinc-700/50 transition-colors ring-1 ring-zinc-700/50 text-zinc-200"
      >
        {{ isRunning ? '⏸' : '▶️' }}
      </button>
      <button
        @click="resetTimer"
        class="px-3 py-2 bg-zinc-800/50 rounded-lg hover:bg-zinc-700/50 transition-colors ring-1 ring-zinc-700/50 text-zinc-200"
      >
        🔄
      </button>
    </div>
  </div>
</template>
