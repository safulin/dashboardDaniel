<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary" />
        </ion-buttons>
        <ion-title>🚀 Negocio</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🚀 Negocio</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-grid class="dashboard-grid">

        <!-- Fila 1: 4 KPI SparkLines -->
        <ion-row class="ion-row-1">
          <ion-col size="6" size-lg="3">
            <div class="box"><SparkLine v-bind="sparkUsuarios" /></div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box"><SparkLine v-bind="sparkVisitas" /></div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box"><SparkLine v-bind="sparkFormularios" /></div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box"><SparkLine v-bind="sparkParroquias" /></div>
          </ion-col>
        </ion-row>

        <!-- Fila 2: Mixed Chart + Gauge rating -->
        <ion-row class="ion-row-2">
          <ion-col size="12" size-lg="9">
            <div class="box"><ApexMixedChart :series="mixedSeries" /></div>
          </ion-col>
          <ion-col size="12" size-lg="3">
            <div class="box"><EchartsGauge :value="ratingGauge" title="RATING MEDIO" /></div>
          </ion-col>
        </ion-row>

        <!-- Fila 3: Mapa España + Donut conversión -->
        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="6">
            <div class="box">
              <EchartsMap :data="mapData" title="Usuarios por región" subtitle="KPI: Expansión nacional" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="6">
            <div class="box">
              <ApexDonut :series="donutSeries" :labels="donutLabels" :colors="donutColors" title="Conversión contactos" />
            </div>
          </ion-col>
        </ion-row>

      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonGrid, IonRow, IonCol } from '@ionic/vue'
import { ref } from 'vue'
import SparkLine      from '@/components/SparkLine.vue'
import ApexMixedChart from '@/components/ApexMixedChart.vue'
import ApexDonut      from '@/components/ApexDonut.vue'
import EchartsGauge   from '@/components/EchartsGauge.vue'
import EchartsMap     from '@/components/EchartsMap.vue'

const sparkBase = {
  chartOptions: {
    chart: { type: 'area', sparkline: { enabled: true }, dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.4 } },
    stroke: { curve: 'smooth', width: 2 },
    colors: ['#fff'],
    tooltip: { theme: 'dark', x: { show: false }, y: { title: { formatter: () => '' } } },
  },
}

const sparkUsuarios    = ref({ ...sparkBase, title: 'USUARIOS',    value: '612',    bgColor: 'gradient-blue',   iconName: 'peopleOutline',   chartSeries: [{ data: [310,380,420,390,460,510,612] }] })
const sparkVisitas     = ref({ ...sparkBase, title: 'VISITAS LISTA', value: '58%', bgColor: 'gradient-pink',   iconName: 'listOutline',     chartOptions: {...sparkBase.chartOptions, chart:{...sparkBase.chartOptions.chart,type:'bar'}}, chartSeries: [{ data: [38,42,48,44,51,55,58] }] })
const sparkFormularios = ref({ ...sparkBase, title: 'FORMULARIOS', value: '27%',   bgColor: 'gradient-orange', iconName: 'mailOutline',     chartOptions: {...sparkBase.chartOptions, chart:{...sparkBase.chartOptions.chart,type:'line'}}, chartSeries: [{ data: [12,15,18,21,22,25,27] }] })
const sparkParroquias  = ref({ ...sparkBase, title: 'PARROQUIAS',  value: '14/25', bgColor: 'gradient-green',  iconName: 'businessOutline', chartSeries: [{ data: [6,7,8,9,10,12,14] }] })

const mixedSeries = ref([
  { name: 'Visitas Mapa',  type: 'column', data: [320,380,290,410,370,430,480,395,440,500,462] },
  { name: 'Visitas Lista', type: 'area',   data: [210,260,230,310,280,340,360,290,330,380,350] },
  { name: 'Formularios',   type: 'line',   data: [48,62,55,78,70,85,91,74,82,95,88]            },
])

const ratingGauge = ref(73)

const mapData = ref([
  { name: 'Cataluña',          value: 480 },
  { name: 'Madrid',            value: 210 },
  { name: 'Valencia',          value: 95  },
  { name: 'Andalucía',         value: 62  },
  { name: 'País Vasco',        value: 44  },
  { name: 'Galicia',           value: 35  },
  { name: 'Aragón',            value: 22  },
  { name: 'Castilla y León',   value: 18  },
  { name: 'Castilla-La Mancha',value: 12  },
  { name: 'Asturias',          value: 9   },
  { name: 'Murcia',            value: 8   },
  { name: 'Navarra',           value: 7   },
  { name: 'Extremadura',       value: 5   },
  { name: 'Cantabria',         value: 4   },
  { name: 'La Rioja',          value: 3   },
  { name: 'Islas Baleares',    value: 6   },
  { name: 'Canarias',          value: 11  },
])

const donutSeries = ref([27, 43, 30])
const donutLabels = ref(['Formulario enviado', 'Solo detalles', 'Abandono'])
const donutColors = ref(['#42d77d', '#4A90D9', '#e04055'])
</script>

<style scoped>
ion-row { overflow: hidden; }
ion-col { max-height: 100%; --ion-grid-column-padding: 8px; }
.box {
  background: #1E1E1E; height: 100%; max-height: 100%; overflow: hidden;
  border-radius: 10px; display: flex; flex-direction: column;
  justify-content: center; align-items: center;
  border: 1px solid rgba(255,255,255,0.05);
}
@media (min-width: 992px) {
  ion-grid { height: 100%; }
  .ion-row-1 { height: 20%; max-height: 20%; }
  .ion-row-2 { height: 40%; max-height: 40%; }
  .ion-row-3 { height: 40%; max-height: 40%; }
}
</style>
