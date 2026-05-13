<template>
  <ion-app>
    <ion-split-pane content-id="main-content">
      <ion-menu content-id="main-content" type="overlay">
        <ion-content>
          <ion-list id="inbox-list">
            <div class="menu-header">
              <div class="menu-logo">
                <ion-icon :icon="flowerOutline" class="logo-icon" />
              </div>
              <ion-list-header>Rosario Flores</ion-list-header>
              <ion-note>Dashboard · Data Viz</ion-note>
            </div>

            <ion-menu-toggle :auto-hide="false" v-for="(p, i) in appPages" :key="i">
              <ion-item
                @click="selectedIndex = i"
                router-direction="root"
                :router-link="p.url"
                lines="none"
                :detail="false"
                class="hydrated"
                :class="{ selected: selectedIndex === i }"
              >
                <ion-icon aria-hidden="true" slot="start" :ios="p.iosIcon" :md="p.mdIcon" />
                <ion-label>{{ p.title }}</ion-label>
              </ion-item>
            </ion-menu-toggle>
          </ion-list>

          <div class="menu-footer">
            <ion-note>App · Parròquies de BCN</ion-note>
          </div>
        </ion-content>
      </ion-menu>
      <ion-router-outlet id="main-content" />
    </ion-split-pane>
  </ion-app>
</template>

<script setup lang="ts">
import {
  IonApp, IonContent, IonIcon, IonItem, IonLabel, IonList,
  IonListHeader, IonMenu, IonMenuToggle, IonNote, IonRouterOutlet, IonSplitPane,
} from '@ionic/vue'
import {
  rocketOutline, rocketSharp, pulseOutline, pulseSharp,
  speedometerOutline, speedometerSharp, flowerOutline,
} from 'ionicons/icons'
import { ref, onMounted, watch } from 'vue'
import { useRoute } from 'vue-router'

const selectedIndex = ref(0)
const appPages = [
  { title: 'Negocio', url: '/negocio', iosIcon: rocketOutline,     mdIcon: rocketSharp      },
  { title: 'Técnico', url: '/tecnico', iosIcon: pulseOutline,      mdIcon: pulseSharp       },
  { title: 'KPIs',    url: '/kpis',    iosIcon: speedometerOutline, mdIcon: speedometerSharp },
]

const route = useRoute()
const updateSelectedIndex = () => {
  const idx = appPages.findIndex(p => p.url === route.path)
  if (idx !== -1) selectedIndex.value = idx
}
onMounted(updateSelectedIndex)
watch(route, updateSelectedIndex)
</script>

<style scoped>
ion-split-pane { --side-max-width: 280px; }

ion-menu ion-content { --background: #0f1923; }

.menu-header {
  padding: 30px 16px 14px 16px;
  display: flex; flex-direction: column; gap: 4px;
  border-bottom: 1px solid rgba(255,255,255,0.08);
  margin-bottom: 10px;
}
.menu-logo {
  width: 44px; height: 44px;
  background: linear-gradient(135deg, #1A3C54, #4A90D9);
  border-radius: 12px;
  display: flex; align-items: center; justify-content: center;
  margin-bottom: 10px;
}
.logo-icon { font-size: 24px; color: white; }

ion-list-header { font-size: 18px; font-weight: 700; color: #fff; padding-left: 0; min-height: auto; margin: 0; }
ion-note { font-size: 12px; color: rgba(255,255,255,0.4); display: inline-block; }

ion-menu ion-item {
  --background: transparent; --color: rgba(255,255,255,0.55);
  --padding-start: 12px; --padding-end: 12px;
  border-radius: 10px; margin: 3px 8px; font-size: 14px; font-weight: 500;
}
ion-menu ion-item ion-icon { color: rgba(255,255,255,0.4); font-size: 20px; }

ion-menu ion-item.selected { --background: rgba(74,144,217,0.18); --color: #4A90D9; }
ion-menu ion-item.selected ion-icon { color: #4A90D9; }

.menu-footer {
  padding: 14px 16px;
  border-top: 1px solid rgba(255,255,255,0.06);
  position: absolute; bottom: 0; width: 100%;
}
#inbox-list { background: transparent; }
</style>
