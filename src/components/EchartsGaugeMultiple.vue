<template>
  <VEChart class="gauge-multi-chart" :option="options" autoresize />
</template>

<script setup lang="ts">
import { ref, watchEffect } from 'vue'
import { use } from 'echarts/core'
import VEChart from 'vue-echarts'
import { GaugeChart } from 'echarts/charts'
import { CanvasRenderer } from 'echarts/renderers'
import { TooltipComponent, LegendComponent } from 'echarts/components'

use([GaugeChart, CanvasRenderer, TooltipComponent, LegendComponent])

interface Segment {
  value: number
  name: string
  color: string
  min?: number
  max?: number
}

const props = defineProps<{ segments: Segment[] }>()

const options = ref({})

watchEffect(() => {
  const totalSegments = props.segments.length
  const radius = ['55%', '70%']

  options.value = {
    backgroundColor: 'transparent',
    tooltip: { formatter: '{a} <br/>{b}: {c}', backgroundColor: '#1E1E1E', textStyle: { color: '#fff' } },
    series: props.segments.map((seg, i) => ({
      type: 'gauge',
      radius: `${85 - i * 22}%`,
      center: ['50%', '55%'],
      startAngle: 90,
      endAngle: -270,
      min: seg.min ?? 0,
      max: seg.max ?? 100,
      pointer: { show: false },
      progress: {
        show: true,
        overlap: false,
        roundCap: true,
        clip: false,
        itemStyle: { color: seg.color },
      },
      axisLine: { lineStyle: { width: 12, color: [[1, 'rgba(255,255,255,0.06)']] } },
      axisTick: { show: false },
      splitLine: { show: false },
      axisLabel: { show: false },
      detail: {
        valueAnimation: true,
        offsetCenter: [0, `${-20 + i * 40}%`],
        fontSize: 13,
        fontWeight: 700,
        color: seg.color,
        formatter: `{value}`,
      },
      title: {
        offsetCenter: [0, `${-4 + i * 40}%`],
        fontSize: 10,
        color: '#8C8C8C',
      },
      data: [{ value: seg.value, name: seg.name }],
    })),
  }
})
</script>

<style scoped>
.gauge-multi-chart {
  width: 100%;
  height: 100%;
  min-height: 200px;
}
</style>
