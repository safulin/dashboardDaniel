<template>
  <div class="chartjs-rt-wrap">
    <p class="chart-title">{{ title }}</p>
    <div class="canvas-container">
      <canvas ref="canvasRef" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import {
  Chart,
  LineController, LineElement, PointElement,
  LinearScale, CategoryScale, Filler, Tooltip,
} from 'chart.js'

Chart.register(LineController, LineElement, PointElement, LinearScale, CategoryScale, Filler, Tooltip)

const props = withDefaults(defineProps<{
  title?: string
  color?: string
  chartType?: 'line' | 'area'
  min?: number
  max?: number
}>(), {
  title: 'Realtime',
  color: '#4A90D9',
  chartType: 'line',
  min: 10,
  max: 90,
})

const canvasRef = ref<HTMLCanvasElement | null>(null)
let chart: Chart | null = null
let interval: ReturnType<typeof setInterval>

const MAX_POINTS = 20

onMounted(() => {
  if (!canvasRef.value) return

  const labels: string[] = Array(MAX_POINTS).fill('')
  const values: number[] = Array(MAX_POINTS).fill(null as any)

  chart = new Chart(canvasRef.value, {
    type: 'line',
    data: {
      labels,
      datasets: [{
        label: props.title,
        borderColor: props.color,
        borderWidth: 2,
        backgroundColor: props.chartType === 'area' ? `${props.color}33` : 'transparent',
        fill: props.chartType === 'area',
        tension: 0.4,
        pointRadius: 0,
        data: values,
      }],
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      animation: { duration: 300 },
      plugins: {
        legend: { display: false },
        tooltip: {
          backgroundColor: '#1E1E1E',
          titleColor: '#aaa',
          bodyColor: '#fff',
        },
      },
      scales: {
        x: {
          ticks: { display: false },
          grid: { color: '#2a2a2a' },
          border: { color: '#333' },
        },
        y: {
          min: 0,
          max: 100,
          ticks: { color: '#666', maxTicksLimit: 5 },
          grid: { color: '#2a2a2a' },
          border: { color: '#333' },
        },
      },
    },
  })

  interval = setInterval(() => {
    if (!chart) return
    const val = Math.floor(Math.random() * (props.max - props.min + 1)) + props.min
    const ds  = chart.data.datasets[0].data as number[]
    ds.push(val)
    if (ds.length > MAX_POINTS) ds.shift()
    ;(chart.data.labels as string[]).push('')
    if ((chart.data.labels as string[]).length > MAX_POINTS)
      (chart.data.labels as string[]).shift()
    chart.update('none')
  }, 1000)
})

onUnmounted(() => {
  clearInterval(interval)
  chart?.destroy()
})
</script>

<style scoped>
.chartjs-rt-wrap {
  width: 100%;
  height: 100%;
  min-height: 180px;
  padding: 14px 16px 10px 16px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

.chart-title {
  font-size: 13px;
  font-weight: 700;
  color: #8C8C8C;
  margin: 0 0 8px 0;
  letter-spacing: 0.3px;
}

.canvas-container {
  flex: 1;
  position: relative;
  min-height: 120px;
}

.canvas-container canvas {
  position: absolute;
  top: 0; left: 0;
  width: 100% !important;
  height: 100% !important;
}
</style>
