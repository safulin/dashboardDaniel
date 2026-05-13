<template>
  <VEChart class="bar-chart" :option="options" autoresize />
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { use } from 'echarts/core'
import VEChart from 'vue-echarts'
import { BarChart } from 'echarts/charts'
import { CanvasRenderer } from 'echarts/renderers'
import { GridComponent, TooltipComponent, TitleComponent } from 'echarts/components'

use([BarChart, CanvasRenderer, GridComponent, TooltipComponent, TitleComponent])

const props = withDefaults(defineProps<{
  categories: string[]
  values: number[]
  title?: string
  color?: string
  target?: number
}>(), {
  title: 'Parroquias indexadas',
  color: '#4A90D9',
  target: 25,
})

const options = computed(() => ({
  backgroundColor: 'transparent',
  title: {
    text: props.title,
    left: 'left',
    textStyle: { fontSize: '14px', fontWeight: '700', color: '#8C8C8C' },
  },
  tooltip: { trigger: 'axis', theme: 'dark', backgroundColor: '#1E1E1E', textStyle: { color: '#fff' } },
  grid: { left: '3%', right: '8%', bottom: '3%', containLabel: true },
  xaxis: { type: 'value', max: props.target, axisLabel: { color: '#8C8C8C' }, splitLine: { lineStyle: { color: '#333' } } },
  yAxis: { type: 'category', data: props.categories, axisLabel: { color: '#aaa', fontSize: 11 } },
  xAxis: {
    type: 'value',
    max: props.target,
    axisLabel: { color: '#8C8C8C' },
    splitLine: { lineStyle: { color: '#2a2a2a' } },
    axisLine: { lineStyle: { color: '#444' } },
  },
  series: [{
    type: 'bar',
    data: props.values.map((v, i) => ({
      value: v,
      itemStyle: {
        color: v >= props.target * 0.8 ? '#42d77d' : v >= props.target * 0.5 ? '#ffd166' : props.color,
        borderRadius: [0, 4, 4, 0],
      },
    })),
    label: {
      show: true,
      position: 'right',
      color: '#aaa',
      fontSize: 11,
      formatter: '{c}',
    },
    markLine: {
      data: [{ xAxis: props.target }],
      symbol: 'none',
      lineStyle: { color: '#ffd166', type: 'dashed', width: 2 },
      label: { formatter: `KPI: ${props.target}`, color: '#ffd166', fontSize: 11 },
    },
  }],
}))
</script>

<style scoped>
.bar-chart {
  width: 100%;
  height: 100%;
  min-height: 220px;
}
</style>
