<template>
  <div class="mixed-chart-wrap">
    <VueApexCharts type="line" height="100%" :options="mergedOptions" :series="series" />
  </div>
</template>

<script setup lang="ts">
import VueApexCharts from 'vue3-apexcharts'
import { computed } from 'vue'

const props = withDefaults(defineProps<{
  series: any[]
  options?: any
}>(), {
  options: () => ({}),
})

const defaultOptions = {
  chart: {
    type: 'line',
    stacked: false,
    background: 'transparent',
    toolbar: { show: false },
    animations: { enabled: true },
  },
  theme: { mode: 'dark', palette: 'palette1' },
  title: {
    text: 'Visitas por sección',
    align: 'left',
    style: { fontSize: '14px', fontWeight: '700', color: '#8C8C8C' },
  },
  stroke: { width: [0, 2, 3], curve: 'smooth' },
  plotOptions: { bar: { columnWidth: '45%' } },
  fill: {
    opacity: [0.85, 0.25, 1],
    gradient: {
      inverseColors: false, shade: 'light', type: 'vertical',
      opacityFrom: 0.85, opacityTo: 0.55, stops: [0, 100, 100, 100],
    },
  },
  labels: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago', 'Sep', 'Oct', 'Nov'],
  xaxis: { labels: { style: { colors: '#8C8C8C' } } },
  yaxis: {
    labels: { style: { colors: '#8C8C8C' } },
    tooltip: { enabled: true },
  },
  grid: { show: true, borderColor: '#333' },
  legend: { position: 'top', labels: { colors: '#aaa' } },
  tooltip: {
    shared: true, theme: 'dark', intersect: false,
    y: { formatter: (v: number) => v !== null ? `${v.toFixed(0)}` : '' },
  },
  markers: { size: 0 },
  colors: ['#4A90D9', '#10B981', '#F97316'],
}

const mergedOptions = computed(() => ({ ...defaultOptions, ...props.options }))
</script>

<style scoped>
.mixed-chart-wrap {
  width: 100%;
  height: 100%;
  min-height: 260px;
  padding: 16px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}
</style>
