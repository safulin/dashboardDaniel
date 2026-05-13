<template>
  <div class="donut-wrap">
    <VueApexCharts type="donut" height="100%" :options="options" :series="series" />
  </div>
</template>

<script setup lang="ts">
import VueApexCharts from 'vue3-apexcharts'

const props = defineProps<{
  series: number[]
  labels: string[]
  title?: string
  colors?: string[]
}>()

const options = {
  chart: { background: 'transparent', toolbar: { show: false } },
  theme: { mode: 'dark' },
  labels: props.labels,
  colors: props.colors ?? ['#4A90D9', '#10B981', '#F97316', '#e04055'],
  title: {
    text: props.title ?? 'Distribución',
    align: 'left',
    style: { fontSize: '14px', fontWeight: '700', color: '#8C8C8C' },
  },
  legend: {
    position: 'bottom',
    labels: { colors: '#aaa' },
  },
  dataLabels: {
    style: { fontSize: '12px', colors: ['#fff'] },
  },
  plotOptions: {
    pie: {
      donut: {
        size: '65%',
        labels: {
          show: true,
          total: {
            show: true,
            label: 'Total',
            color: '#aaa',
            fontSize: '13px',
            formatter: (w: any) => w.globals.seriesTotals.reduce((a: number, b: number) => a + b, 0),
          },
        },
      },
    },
  },
  stroke: { width: 0 },
  tooltip: { theme: 'dark' },
}
</script>

<style scoped>
.donut-wrap {
  width: 100%;
  height: 100%;
  min-height: 260px;
  padding: 16px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}
</style>
