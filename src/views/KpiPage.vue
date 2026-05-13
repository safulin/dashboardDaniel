<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary" />
        </ion-buttons>
        <ion-title>🎯 KPIs</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🎯 KPIs</ion-title>
        </ion-toolbar>
      </ion-header>

      <h1 class="ion-padding section-title">🚀 KPIs de Negocio</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion v-for="item in kpisNegocio" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p class="kpi-description">{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(el, idx) in item.smart" :key="idx">
                <ion-label>
                  <span class="smart-letter">{{ el.letter }}</span>
                  <span class="smart-arrow"> → </span>{{ el.content }}
                </ion-label>
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>

      <br />

      <h1 class="ion-padding section-title">📈 KPIs Técnicos</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion v-for="item in kpisTecnico" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p class="kpi-description">{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(el, idx) in item.smart" :key="idx">
                <ion-label>
                  <span class="smart-letter">{{ el.letter }}</span>
                  <span class="smart-arrow"> → </span>{{ el.content }}
                </ion-label>
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>
      <br /><br />
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonAccordionGroup, IonAccordion, IonItem, IonLabel, IonList } from '@ionic/vue'
import { ref } from 'vue'

interface SmartElement { letter: string; content: string }
interface SmartGoal { id: number; title: string; description: string; smart: SmartElement[] }

const kpisNegocio = ref<SmartGoal[]>([
  { id: 1, title: 'Aumentar usuarios registrados',
    description: 'Incrementar el número de usuarios registrados de 500 a 1.000 en los próximos 6 meses, mediante campañas de difusión en parroquias asociadas, con el objetivo de ampliar la comunidad activa.',
    smart: [
      { letter: 'S', content: 'Aumentar usuarios registrados en la app' },
      { letter: 'M', content: 'De 500 a 1.000 usuarios' },
      { letter: 'A', content: 'Mediante campañas de difusión en parroquias asociadas' },
      { letter: 'R', content: 'Para ampliar la comunidad activa de la plataforma' },
      { letter: 'T', content: 'En los próximos 6 meses' },
    ]},
  { id: 2, title: 'Uso semanal de la sección Lista',
    description: 'Conseguir que el 60% de los usuarios activos visiten la sección Lista al menos una vez por semana durante el próximo trimestre, mediante notificaciones push con novedades de parroquias.',
    smart: [
      { letter: 'S', content: 'Que los usuarios activos visiten la sección Lista' },
      { letter: 'M', content: '60% de usuarios activos, mínimo 1 vez/semana' },
      { letter: 'A', content: 'Con notificaciones push semanales con novedades' },
      { letter: 'R', content: 'Para mejorar la visibilidad del catálogo de parroquias' },
      { letter: 'T', content: 'Durante el próximo trimestre' },
    ]},
  { id: 3, title: 'Tasa de conversión Detalles → Formulario',
    description: 'Lograr que el 30% de los usuarios que acceden a la página de detalles envíen el formulario de contacto en los próximos 2 meses, optimizando el botón "CONTÁCTANOS".',
    smart: [
      { letter: 'S', content: 'Que usuarios que ven detalles envíen el formulario' },
      { letter: 'M', content: '30% de conversión detalles → formulario' },
      { letter: 'A', content: 'Rediseñando el CTA "CONTÁCTANOS" para mayor visibilidad' },
      { letter: 'R', content: 'Para aumentar el contacto entre fieles y parroquias' },
      { letter: 'T', content: 'En los próximos 2 meses' },
    ]},
  { id: 4, title: 'Ampliar catálogo de parroquias',
    description: 'Aumentar las parroquias indexadas de 6 a 25 antes de finalizar el primer trimestre, estableciendo acuerdos con el Arxidiòcesi de Barcelona.',
    smart: [
      { letter: 'S', content: 'Ampliar el número de parroquias indexadas' },
      { letter: 'M', content: 'De 6 a 25 parroquias' },
      { letter: 'A', content: 'Estableciendo acuerdos con el Arxidiòcesi de Barcelona' },
      { letter: 'R', content: 'Para ofrecer cobertura completa de la ciudad' },
      { letter: 'T', content: 'Antes de finalizar el primer trimestre del año' },
    ]},
  { id: 5, title: 'Rating promedio ≥ 4.5 estrellas',
    description: 'Alcanzar rating ≥ 4.5 estrellas en el 80% de las parroquias al finalizar el segundo trimestre, con recordatorio de puntuación en la pantalla de agradecimiento.',
    smart: [
      { letter: 'S', content: 'Mejorar el rating promedio de parroquias en la app' },
      { letter: 'M', content: '≥ 4.5 estrellas en el 80% de las parroquias' },
      { letter: 'A', content: 'Con recordatorio de puntuación en pantalla de agradecimiento' },
      { letter: 'R', content: 'Para generar confianza en nuevos usuarios' },
      { letter: 'T', content: 'Al finalizar el segundo trimestre' },
    ]},
])

const kpisTecnico = ref<SmartGoal[]>([
  { id: 1, title: 'Tiempo de carga inicial < 2s',
    description: 'Mantener el tiempo de carga inicial por debajo de 2 segundos en el 95% de sesiones durante el próximo mes, aplicando code splitting y lazy loading de vistas.',
    smart: [
      { letter: 'S', content: 'Mantener tiempo de carga inicial bajo' },
      { letter: 'M', content: '< 2s en el 95% de sesiones' },
      { letter: 'A', content: 'Aplicando code splitting y lazy loading de vistas' },
      { letter: 'R', content: 'Para mejorar la experiencia desde el primer acceso' },
      { letter: 'T', content: 'Durante el próximo mes' },
    ]},
  { id: 2, title: 'Reducir tasa de error en el mapa',
    description: 'Reducir los errores en peticiones del mapa Leaflet del 8% al 2% en 30 días, implementando reintentos automáticos y toasts de error.',
    smart: [
      { letter: 'S', content: 'Reducir errores en peticiones del mapa Leaflet' },
      { letter: 'M', content: 'Del 8% al 2% de tasa de error' },
      { letter: 'A', content: 'Implementando reintentos automáticos y toasts de aviso' },
      { letter: 'R', content: 'El mapa es la pantalla principal de la app' },
      { letter: 'T', content: 'En los próximos 30 días' },
    ]},
  { id: 3, title: 'Rendimiento del cálculo de rutas',
    description: 'Asegurar que el 90% de rutas (pie, bus, coche) se resuelven en < 3s en los próximos 2 meses, evaluando servidor de routing propio.',
    smart: [
      { letter: 'S', content: 'Que las rutas calculadas se resuelvan rápidamente' },
      { letter: 'M', content: '90% de rutas resueltas en < 3s' },
      { letter: 'A', content: 'Evaluando servidor de routing propio vs servicio externo' },
      { letter: 'R', content: 'Las rutas son una funcionalidad core de la app' },
      { letter: 'T', content: 'En los próximos 2 meses' },
    ]},
  { id: 4, title: 'Sesiones simultáneas en hora punta',
    description: 'Mantener ≥ 50 sesiones simultáneas en hora punta (9-11h y 18-20h) a lo largo del próximo mes, monitorizando con herramientas de analítica en tiempo real.',
    smart: [
      { letter: 'S', content: 'Mantener sesiones activas simultáneas en hora punta' },
      { letter: 'M', content: '≥ 50 sesiones en franjas 9-11h y 18-20h' },
      { letter: 'A', content: 'Monitorizando con analítica en tiempo real' },
      { letter: 'R', content: 'Para garantizar que la infraestructura soporta la demanda' },
      { letter: 'T', content: 'Durante el próximo mes' },
    ]},
  { id: 5, title: 'Reducir tamaño del bundle',
    description: 'Reducir el bundle de 2.1 MB a < 1.5 MB antes del próximo release, eliminando dependencias no usadas y aplicando tree shaking sobre Leaflet e Ionic.',
    smart: [
      { letter: 'S', content: 'Reducir el tamaño del bundle de producción' },
      { letter: 'M', content: 'De 2.1 MB a < 1.5 MB' },
      { letter: 'A', content: 'Auditando dependencias y tree shaking en Leaflet e Ionic' },
      { letter: 'R', content: 'Un bundle más pequeño mejora la carga en móviles' },
      { letter: 'T', content: 'Antes del próximo release' },
    ]},
])
</script>

<style scoped>
.section-title { font-size: 20px; font-weight: 700; color: #ffffff; margin-top: 8px; }
.kpi-description { font-size: 13px; line-height: 1.6; color: rgba(255,255,255,0.65); margin-bottom: 12px; }
.smart-letter { font-weight: 800; color: var(--ion-color-primary); font-size: 14px; }
.smart-arrow  { color: rgba(255,255,255,0.3); }

ion-accordion.accordion-collapsing ion-item[slot='header'],
ion-accordion.accordion-collapsed  ion-item[slot='header'] {
  --background: var(--ion-color-light);
  --color: var(--ion-color-light-contrast);
}
ion-accordion.accordion-expanding ion-item[slot='header'],
ion-accordion.accordion-expanded  ion-item[slot='header'] {
  --background: rgba(var(--ion-color-primary-rgb), 0.14);
  --color: var(--ion-color-primary);
}
</style>
