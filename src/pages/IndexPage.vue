<template>
  <q-page class="dashboard-page q-pa-md q-pa-lg-lg">
    <div class="dashboard-header q-mb-lg">
      <div class="text-overline text-cyan-4">Simulador de Productividad</div>
      <h1 class="text-h4 text-h3-md text-weight-bold q-my-xs">
        Panel de enfoque académico
      </h1>
      <p class="text-subtitle2 text-grey-5 q-mb-none">
        Visualiza tu sesión Pomodoro y una métrica de rendimiento en tiempo real.
      </p>
    </div>

    <div class="row q-col-gutter-md">
      <div class="col-12 col-lg-7">
        <PomodoroTimer
          :remaining-seconds="remainingSeconds"
          :total-seconds="pomodoroSeconds"
          :is-running="isRunning"
          :is-finished="isFinished"
          @start="startTimer"
          @pause="pauseTimer"
          @reset="resetTimer"
        />
      </div>

      <div class="col-12 col-lg-5">
        <FocusStatus
          :focus-level="focusLevel"
          :status-message="statusMessage"
        />
      </div>

      <div class="col-12">
        <ProductivityChart
          :labels="chartLabels"
          :values="chartValues"
        />
      </div>
    </div>

    <div class="row items-center q-col-gutter-md q-mt-md">
      <div class="col-auto">
        <q-toggle
          v-model="soundEnabled"
          color="positive"
          checked-icon="volume_up"
          unchecked-icon="volume_off"
          label="Sonido al finalizar"
        />
      </div>
      <div class="col-auto">
        <q-chip square color="dark" text-color="cyan-3" icon="insights">
          Sesiones completadas: {{ completedSessions }}
        </q-chip>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { computed, onBeforeUnmount, ref } from 'vue'
import FocusStatus from 'components/FocusStatus.vue'
import PomodoroTimer from 'components/PomodoroTimer.vue'
import ProductivityChart from 'components/ProductivityChart.vue'

const pomodoroSeconds = 25 * 60
const maxChartPoints = 14

const remainingSeconds = ref(pomodoroSeconds)
const isRunning = ref(false)
const isFinished = ref(false)
const focusLevel = ref(82)
const soundEnabled = ref(true)
const completedSessions = ref(0)

const chartLabels = ref(['T1'])
const chartValues = ref([focusLevel.value])

// Intervalos separados para reloj de cuenta regresiva y serie de gráfica.
let timerInterval = null
let chartInterval = null
let tick = 1

// Mensaje dinámico principal del estado visual.
const statusMessage = computed(() => {
  if (isFinished.value) {
    return 'Descanso recomendado'
  }

  if (focusLevel.value >= 75) {
    return 'Enfocado'
  }

  if (focusLevel.value >= 45) {
    return 'Distracción detectada'
  }

  return 'Descanso recomendado'
})

const clamp = (value, min, max) => Math.max(min, Math.min(max, value))

const updateFocus = () => {
  if (!isRunning.value) {
    return
  }

  // Simulación simple: fatiga progresiva + variación aleatoria controlada.
  const fatigue = 1 - remainingSeconds.value / pomodoroSeconds
  const randomShift = Math.floor(Math.random() * 11) - 5
  const nextFocus = focusLevel.value + randomShift - fatigue * 3
  focusLevel.value = clamp(Math.round(nextFocus), 20, 100)
}

const pushChartPoint = () => {
  tick += 1
  chartLabels.value.push(`T${tick}`)
  chartValues.value.push(focusLevel.value)

  if (chartLabels.value.length > maxChartPoints) {
    chartLabels.value.shift()
    chartValues.value.shift()
  }
}

const playFinishSound = () => {
  if (!soundEnabled.value) {
    return
  }

  // Beep corto generado por Web Audio API (sin assets externos).
  const audioContext = new window.AudioContext()
  const oscillator = audioContext.createOscillator()
  const gainNode = audioContext.createGain()

  oscillator.type = 'sine'
  oscillator.frequency.value = 880
  oscillator.connect(gainNode)
  gainNode.connect(audioContext.destination)
  gainNode.gain.value = 0.08

  oscillator.start()
  oscillator.stop(audioContext.currentTime + 0.25)
}

const stopIntervals = () => {
  if (timerInterval) {
    window.clearInterval(timerInterval)
    timerInterval = null
  }

  if (chartInterval) {
    window.clearInterval(chartInterval)
    chartInterval = null
  }
}

const startTimer = () => {
  if (isRunning.value || isFinished.value) {
    return
  }

  // Inicio de sesión: activa contador por segundo.
  isRunning.value = true

  timerInterval = window.setInterval(() => {
    if (remainingSeconds.value <= 1) {
      remainingSeconds.value = 0
      isRunning.value = false
      isFinished.value = true
      completedSessions.value += 1
      stopIntervals()
      playFinishSound()
      return
    }

    remainingSeconds.value -= 1
    updateFocus()
  }, 1000)

  chartInterval = window.setInterval(() => {
    updateFocus()
    pushChartPoint()
  }, 3000)
}

const pauseTimer = () => {
  isRunning.value = false
  stopIntervals()
}

const resetTimer = () => {
  stopIntervals()
  isRunning.value = false
  isFinished.value = false
  remainingSeconds.value = pomodoroSeconds
  focusLevel.value = 82
  tick = 1
  chartLabels.value = ['T1']
  chartValues.value = [focusLevel.value]
}

onBeforeUnmount(() => {
  stopIntervals()
})
</script>

<style scoped>
.dashboard-page {
  min-height: 100%;
}

.dashboard-header h1 {
  text-shadow: 0 0 25px rgba(0, 229, 255, 0.24);
}
</style>
