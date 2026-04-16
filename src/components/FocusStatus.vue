<template>
  <q-card class="glass-card">
    <q-card-section class="row items-center justify-between">
      <div>
        <div class="text-subtitle2 text-grey-4">Estado visual</div>
        <div class="text-h6 text-weight-medium">{{ statusMessage }}</div>
      </div>
      <q-icon :name="statusIcon" size="34px" :color="statusColor" />
    </q-card-section>

    <q-card-section>
      <div class="row items-center justify-between q-mb-sm">
        <span class="text-caption text-grey-5">Nivel de enfoque</span>
        <span class="text-subtitle2 text-weight-bold" :class="`text-${statusColor}`">
          {{ focusLevel }}%
        </span>
      </div>

      <q-linear-progress
        rounded
        size="10px"
        :value="focusLevel / 100"
        :color="statusColor"
        track-color="dark-page"
      />
    </q-card-section>
  </q-card>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  focusLevel: {
    type: Number,
    default: 50
  },
  statusMessage: {
    type: String,
    default: 'Enfocado'
  }
})

const statusColor = computed(() => {
  if (props.statusMessage === 'Enfocado') {
    return 'positive'
  }

  if (props.statusMessage === 'Distracción detectada') {
    return 'warning'
  }

  return 'info'
})

const statusIcon = computed(() => {
  if (props.statusMessage === 'Enfocado') {
    return 'psychology'
  }

  if (props.statusMessage === 'Distracción detectada') {
    return 'notification_important'
  }

  return 'self_improvement'
})
</script>
