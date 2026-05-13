<template>
  <VEChart class="gauge-chart" :option="options" autoresize />
</template>

<script setup lang="ts">
import { ref, watchEffect } from 'vue'
import { use } from 'echarts/core'
import VEChart from 'vue-echarts'
import { GaugeChart } from 'echarts/charts'
import { CanvasRenderer } from 'echarts/renderers'
import { TooltipComponent } from 'echarts/components'

use([GaugeChart, CanvasRenderer, TooltipComponent])

const props = defineProps<{ value: number; title?: string }>()

const options = ref({})

watchEffect(() => {
  options.value = {
    backgroundColor: 'transparent',
    series: [{
      type: 'gauge',
      center: ['50%', '58%'],
      radius: '90%',
      min: 0, max: 100,
      axisLine: {
        lineStyle: {
          width: 18,
          color: [[0.2, '#e04055'], [0.69, '#ffd166'], [1, '#42d77d']],
        },
      },
      pointer: { itemStyle: { color: 'auto' }, length: '58%' },
      axisTick: {
        distance: -18, length: 4,
        lineStyle: { color: '#fff', width: 1 },
      },
      splitLine: {
        distance: -18, length: 18,
        lineStyle: { color: '#fff', width: 2 },
      },
      axisLabel: {
        color: 'inherit', distance: 26, fontSize: 13,
        formatter: (v: number) => v % 20 === 0 ? String(v) : '',
      },
      detail: {
        valueAnimation: true,
        formatter: '{value}',
        color: 'inherit',
        fontSize: 22,
        offsetCenter: [0, '20%'],
      },
      data: [{
        value: props.value,
        name: props.title ?? 'KPI',
        title: { color: '#8C8C8C', offsetCenter: [0, '70%'], fontSize: 12 },
      }],
    }],
  }
})
</script>

<style scoped>
.gauge-chart {
  width: 100%;
  height: 100%;
  min-height: 200px;
}
</style>
