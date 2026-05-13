<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary" />
        </ion-buttons>
        <ion-title>📈 Técnico</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">📈 Técnico</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-grid class="dashboard-grid">

        <!-- Fila 1: 3 SparkLines -->
        <ion-row class="ion-row-1">
          <ion-col size="12" size-lg="4">
            <div class="box"><SparkLine v-bind="sparkSesiones" /></div>
          </ion-col>
          <ion-col size="6" size-lg="4">
            <div class="box"><SparkLine v-bind="sparkErrores" /></div>
          </ion-col>
          <ion-col size="6" size-lg="4">
            <div class="box"><SparkLine v-bind="sparkCarga" /></div>
          </ion-col>
        </ion-row>

        <!-- Fila 2: Gauge online + Línea realtime (push/pull) -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-md="3" push-md="9">
            <div class="box">
              <EchartsGauge :value="currentValue" title="USUARIOS ONLINE" />
            </div>
          </ion-col>
          <ion-col size="12" size-md="9" pull-md="3">
            <div class="box">
              <ApexLineRT :series="series" title="Usuarios online en tiempo real" :kpi-target="50" color="#4A90D9" />
            </div>
          </ion-col>
        </ion-row>

        <!-- Fila 3: CPU + Memoria + Gauge múltiple rutas -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="4.5">
            <div class="box">
              <ChartJSLineAreaRT chartType="line" title="Carga CPU" color="#10B981" :min="10" :max="85" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="4.5">
            <div class="box">
              <ChartJSLineAreaRT chartType="area" title="Uso Memoria" color="#4A90D9" :min="50" :max="78" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="3">
            <div class="box">
              <EchartsGaugeMultiple :segments="ringSegments" />
            </div>
          </ion-col>
        </ion-row>

      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonGrid, IonRow, IonCol } from '@ionic/vue'
import { ref, onMounted, onUnmounted } from 'vue'

import SparkLine            from '@/components/SparkLine.vue'
import ApexLineRT           from '@/components/ApexLineRT.vue'
import EchartsGauge         from '@/components/EchartsGauge.vue'
import EchartsGaugeMultiple from '@/components/EchartsGaugeMultiple.vue'
import ChartJSLineAreaRT    from '@/components/ChartJSLineAreaRT.vue'

// ── Constantes ────────────────────────────────────────
const UPDATE_INTERVAL  = 1000
const MAX_DATA_POINTS  = 60
let lastDate = Date.now()
let interval: ReturnType<typeof setInterval>

// ── SparkLines ────────────────────────────────────────
const sparkBase = {
  chartOptions: {
    chart: { type: 'area', sparkline: { enabled: true }, dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.4 } },
    stroke: { curve: 'smooth', width: 2 },
    colors: ['#fff'],
    tooltip: { theme: 'dark', x: { show: false }, y: { title: { formatter: () => '' } } },
  },
}

const sparkSesiones = ref({ ...sparkBase, title: 'SESIONES',    value: '47',    bgColor: 'gradient-blue',   iconName: 'flashOutline',       chartSeries: [{ data: [28,33,41,38,44,42,47] }] })
const sparkErrores  = ref({ ...sparkBase, title: 'TASA ERROR',  value: '3.2%',  bgColor: 'gradient-pink',   iconName: 'warningOutline',     chartOptions: {...sparkBase.chartOptions, chart:{...sparkBase.chartOptions.chart,type:'bar'}},  chartSeries: [{ data: [8.1,7.2,6.8,5.5,4.9,4.1,3.2] }] })
const sparkCarga    = ref({ ...sparkBase, title: 'CARGA MEDIA', value: '1.8s',  bgColor: 'gradient-orange', iconName: 'timerOutline',       chartOptions: {...sparkBase.chartOptions, chart:{...sparkBase.chartOptions.chart,type:'line'}}, chartSeries: [{ data: [2.8,2.5,2.3,2.1,2.0,1.9,1.8] }] })

// ── Realtime data ─────────────────────────────────────
const data   = ref<{ x: number; y: number }[]>([])
const series = ref([{ name: 'Usuarios', data: data.value }])

const currentValue = ref(0)

const ringSegments = ref([
  { value: 0, name: '🚶 A pie',   color: '#4A90D9', min: 60, max: 95 },
  { value: 0, name: '🚌 Bus',     color: '#10B981', min: 70, max: 98 },
  { value: 0, name: '🚗 Coche',   color: '#F97316', min: 55, max: 90 },
])

// ── Lógica realtime ───────────────────────────────────
function addDataRealTime() {
  const newX = lastDate + UPDATE_INTERVAL
  const newY = Math.floor(Math.random() * 80) + 15
  data.value.push({ x: newX, y: newY })

  if (data.value.length > MAX_DATA_POINTS) {
    data.value = data.value.slice(-2)
    series.value = [{ name: 'Usuarios', data: data.value }]
  }

  lastDate = newX
  currentValue.value = newY

  ringSegments.value.forEach(s => {
    s.value = Math.floor(Math.random() * (s.max - s.min + 1)) + s.min
  })
}

onMounted(()  => { interval = setInterval(addDataRealTime, UPDATE_INTERVAL) })
onUnmounted(() => { clearInterval(interval) })
</script>

<style scoped>
ion-row { overflow: hidden; }
ion-col { max-height: 100%; --ion-grid-column-padding: 8px; }
.box {
  background: #1E1E1E; height: 100%; max-height: 100%; overflow: hidden;
  border-radius: 10px; display: flex; flex-direction: column;
  justify-content: center; align-items: start;
  border: 1px solid rgba(255,255,255,0.05);
}
@media (min-width: 992px) {
  ion-grid { height: 100%; }
  .ion-row-1 { height: 20%; max-height: 20%; }
  .ion-row-2 { height: 50%; max-height: 50%; }
  .ion-row-3 { height: 30%; max-height: 30%; }
}
</style>
