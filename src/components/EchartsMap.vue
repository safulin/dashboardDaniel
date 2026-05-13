<template>
  <VEChart class="map-chart" :option="options" autoresize />
</template>

<script setup lang="ts">
import { ref, watchEffect, onMounted } from 'vue'
import * as echarts from 'echarts/core'
import VEChart from 'vue-echarts'
import { MapChart } from 'echarts/charts'
import { CanvasRenderer } from 'echarts/renderers'
import {
  TooltipComponent, VisualMapComponent,
  TitleComponent, ToolboxComponent,
} from 'echarts/components'
import spainGeoJSON from '@/assets/spain.geo.json'

echarts.use([MapChart, CanvasRenderer, TooltipComponent, VisualMapComponent, TitleComponent, ToolboxComponent])

const props = withDefaults(defineProps<{
  title?: string
  subtitle?: string
  data: { name: string; value: number }[]
}>(), {
  title: 'Usuarios por CCAA',
  subtitle: 'KPI: Expansión nacional',
  data: () => [],
})

const mapReady = ref(false)
const options  = ref({})

onMounted(() => {
  echarts.registerMap('spain', spainGeoJSON as any)
  mapReady.value = true
})

watchEffect(() => {
  if (!mapReady.value) return

  options.value = {
    backgroundColor: 'transparent',
    title: {
      text: props.title,
      subtext: props.subtitle,
      left: 'left',
      textStyle:    { color: '#8C8C8C', fontSize: 14, fontWeight: 'bold' },
      subtextStyle: { color: '#555' },
    },
    tooltip: {
      trigger: 'item',
      backgroundColor: '#1E1E1E',
      borderColor: '#333',
      textStyle: { color: '#fff' },
      formatter: (p: any) => `<b>${p.name}</b><br/>Usuarios: ${p.value ?? 'Sin datos'}`,
    },
    toolbox: {
      show: true,
      right: 10,
      top: 10,
      feature: {
        restore: {},
        saveAsImage: { backgroundColor: '#1E1E1E' },
      },
    },
    visualMap: {
      min: 0,
      max: 500,
      left: 'left',
      bottom: 20,
      text: ['Más', 'Menos'],
      textStyle: { color: '#888' },
      calculable: true,
      inRange:    { color: ['#1A3C54', '#2E6B9E', '#4A90D9', '#8ecae6'] },
      outOfRange: { color: ['rgba(255,255,255,0.04)'] },
    },
    series: [{
      name: 'Usuarios',
      type: 'map',
      map: 'spain',
      roam: true,
      scaleLimit: { min: 1, max: 5 },
      emphasis: {
        label: { show: true, color: '#fff', fontSize: 11 },
        itemStyle: { areaColor: '#4A90D9', borderColor: '#fff', borderWidth: 1 },
      },
      select: {
        itemStyle: { areaColor: '#2E6B9E' },
      },
      itemStyle: {
        areaColor:   'rgba(255,255,255,0.04)',
        borderColor: 'rgba(255,255,255,0.2)',
        borderWidth: 0.8,
      },
      label: {
        show: false,
        color: '#aaa',
        fontSize: 10,
      },
      data: props.data,
    }],
  }
})
</script>

<style scoped>
.map-chart {
  width: 100%;
  height: 100%;
  min-height: 220px;
}
</style>
