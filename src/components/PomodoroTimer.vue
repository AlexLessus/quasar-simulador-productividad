<template>
  <q-card class="glass-card timer-card" :class="{ 'timer-active': isRunning }">
    <q-card-section class="row items-center justify-between">
      <div>
        <div class="text-subtitle2 text-grey-4">Sesión Pomodoro</div>
        <div class="text-h3 text-weight-bold timer-value">{{ formattedTime }}</div>
      </div>
      <q-icon name="timer" size="42px" color="positive" />
    </q-card-section>

    <q-card-section>
      <q-linear-progress
        rounded
        stripe
        size="12px"
        :value="progress"
        color="positive"
        track-color="dark-page"
        class="q-mb-md"
      />

      <div class="row q-gutter-sm">
        <q-btn
          unelevated
          color="positive"
          icon="play_arrow"
          label="Iniciar"
          :disable="isRunning || isFinished"
          @click="$emit('start')"
        />
        <q-btn
          unelevated
          color="info"
          icon="pause"
          label="Pausar"
          :disable="!isRunning"
          @click="$emit('pause')"
        />
        <q-btn
          flat
          color="grey-3"
          icon="restart_alt"
          label="Reiniciar"
          @click="$emit('reset')"
        />
      </div>
    </q-card-section>
  </q-card>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  remainingSeconds: {
    type: Number,
    required: true
  },
  totalSeconds: {
    type: Number,
    default: 1500
  },
  isRunning: {
    type: Boolean,
    default: false
  },
  isFinished: {
    type: Boolean,
    default: false
  }
})

defineEmits(['start', 'pause', 'reset'])

const formattedTime = computed(() => {
  const minutes = Math.floor(props.remainingSeconds / 60)
  const seconds = props.remainingSeconds % 60
  return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`
})

const progress = computed(() => {
  if (props.totalSeconds === 0) {
    return 0
  }

  return props.remainingSeconds / props.totalSeconds
})
</script>

<style scoped>
.timer-card {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.timer-active {
  transform: translateY(-4px) scale(1.01);
  box-shadow: 0 0 30px rgba(0, 255, 178, 0.28);
}

.timer-value {
  letter-spacing: 1px;
}
</style>
