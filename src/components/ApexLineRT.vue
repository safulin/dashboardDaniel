<template>
  <div class="apex-rt-wrap">
    <VueApexCharts type="line" height="100%" :options="chartOptions" :series="series" />
  </div>
</template>

<script setup lang="ts">
import VueApexCharts from 'vue3-apexcharts'

const props = withDefaults(defineProps<{
  series: any[]
  title?: string
  color?: string
  kpiTarget?: number
}>(), {
  title: 'Realtime',
  color: '#4A90D9',
  kpiTarget: 50,
})

const chartOptions = {
  chart: {
    id: 'apex-rt',
    background: 'transparent',
    toolbar: { show: false },
    animations: { enabled: true, easing: 'linear', dynamicAnimation: { speed: 900 } },
  },
  theme: { mode: 'dark' },
  title: {
    text: props.title,
    align: 'left',
    style: { fontSize: '14px', fontWeight: '700', color: '#8C8C8C' },
  },
  colors: [props.color],
  stroke: { curve: 'smooth', width: 2 },
  xaxis: {
    type: 'numeric',
    labels: { show: false },
    axisBorder: { show: false },
    axisTicks: { show: false },
  },
  yaxis: {
    min: 0, max: 100,
    labels: { style: { colors: '#8C8C8C' } },
  },
  annotations: {
    yaxis: [{
      y: props.kpiTarget,
      borderColor: '#ffd166',
      strokeDashArray: 4,
      label: {
        text: `KPI: ${props.kpiTarget}`,
        style: { color: '#ffd166', background: 'transparent', fontSize: '11px' },
      },
    }],
  },
  grid: { borderColor: '#333' },
  tooltip: { theme: 'dark' },
  legend: { show: false },
}
</script>

<style scoped>
.apex-rt-wrap {
  width: 100%;
  height: 100%;
  min-height: 220px;
  padding: 16px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}
</style>
