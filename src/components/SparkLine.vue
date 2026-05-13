<template>
  <div class="sparkline-card" :class="bgColor">
    <div class="spark-top">
      <div class="spark-info">
        <span class="spark-title">{{ title }}</span>
        <span class="spark-value">{{ value }}</span>
      </div>
      <ion-icon :icon="ionicons[iconName] || ionicons['statsChartOutline']" class="spark-icon" />
    </div>
    <VueApexCharts
      :type="chartOptions.chart.type"
      height="60"
      :options="chartOptions"
      :series="chartSeries"
    />
  </div>
</template>

<script setup lang="ts">
import VueApexCharts from 'vue3-apexcharts'
import { IonIcon } from '@ionic/vue'
import * as allIcons from 'ionicons/icons'

const ionicons: Record<string, string> = allIcons as any

const props = withDefaults(defineProps<{
  title: string
  value: string
  bgColor: string
  textColor?: string
  iconName: string
  chartOptions: any
  chartSeries: any[]
}>(), {
  textColor: 'white',
})
</script>

<style scoped>
.sparkline-card {
  width: 100%;
  height: 100%;
  padding: 16px 16px 6px 16px;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  overflow: hidden;
  box-sizing: border-box;
}

.spark-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.spark-info {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.spark-title {
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 1.2px;
  opacity: 0.8;
  color: white;
}

.spark-value {
  font-size: 24px;
  font-weight: 800;
  color: white;
  line-height: 1;
}

.spark-icon {
  font-size: 28px;
  color: rgba(255,255,255,0.5);
}

/* Gradientes de fondo */
.gradient-blue {
  background: linear-gradient(135deg, #1A3C54 0%, #4A90D9 100%);
}
.gradient-pink {
  background: linear-gradient(135deg, #6B1F5C 0%, #D9509A 100%);
}
.gradient-orange {
  background: linear-gradient(135deg, #7A3000 0%, #F97316 100%);
}
.gradient-green {
  background: linear-gradient(135deg, #064E3B 0%, #10B981 100%);
}
</style>
