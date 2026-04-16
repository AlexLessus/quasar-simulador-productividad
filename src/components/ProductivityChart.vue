<template>
  <q-card class="glass-card">
    <q-card-section class="row items-center justify-between">
      <div>
        <div class="text-subtitle2 text-grey-4">Rendimiento en tiempo real</div>
        <div class="text-h6 text-weight-medium">Tendencia de enfoque</div>
      </div>
      <q-icon name="insights" size="36px" color="info" />
    </q-card-section>

    <q-card-section>
      <div class="chart-wrap">
        <canvas ref="canvasRef" />
      </div>
    </q-card-section>
  </q-card>
</template>

<script setup>
import { Chart, LineController, LineElement, PointElement, LinearScale, CategoryScale, Tooltip, Filler } from 'chart.js'
import { onBeforeUnmount, onMounted, ref, watch } from 'vue'

Chart.register(LineController, LineElement, PointElement, LinearScale, CategoryScale, Tooltip, Filler)

const props = defineProps({
  labels: {
    type: Array,
    default: () => []
  },
  values: {
    type: Array,
    default: () => []
  }
})

const canvasRef = ref(null)
let chart = null

const renderChart = () => {
  if (!canvasRef.value) {
    return
  }

  chart = new Chart(canvasRef.value, {
    type: 'line',
    data: {
      labels: [...props.labels],
      datasets: [
        {
          label: 'Productividad',
          data: [...props.values],
          borderColor: '#00e5ff',
          backgroundColor: 'rgba(0, 229, 255, 0.18)',
          fill: true,
          tension: 0.35,
          pointBackgroundColor: '#00ffb2',
          pointBorderWidth: 0,
          pointRadius: 3
        }
      ]
    },
    options: {
      maintainAspectRatio: false,
      responsive: true,
      animation: {
        duration: 400
      },
      plugins: {
        legend: {
          display: false
        },
        tooltip: {
          backgroundColor: '#0f1722',
          titleColor: '#e2e8f0',
          bodyColor: '#e2e8f0'
        }
      },
      scales: {
        x: {
          ticks: {
            color: '#90a4ae'
          },
          grid: {
            color: 'rgba(148, 163, 184, 0.12)'
          }
        },
        y: {
          min: 0,
          max: 100,
          ticks: {
            color: '#90a4ae'
          },
          grid: {
            color: 'rgba(148, 163, 184, 0.12)'
          }
        }
      }
    }
  })
}

watch(
  () => [props.labels.length, props.values.length, props.values[props.values.length - 1]],
  () => {
    if (!chart) {
      return
    }

    chart.data.labels = [...props.labels]
    chart.data.datasets[0].data = [...props.values]
    chart.update()
  }
)

onMounted(() => {
  renderChart()
})

onBeforeUnmount(() => {
  chart?.destroy()
})
</script>

<style scoped>
.chart-wrap {
  height: 260px;
}
</style>
