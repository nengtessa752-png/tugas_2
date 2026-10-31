<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="primary">
        <ion-title>Cuaca Jakarta</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">
      <h2>🌤️ Prakiraan Suhu Jakarta</h2>
      <p>Data suhu per jam dari Open-Meteo</p>

      <ion-button @click="getWeather">
        Muat Data
      </ion-button>

      <p v-if="error">{{ error }}</p>

      <ion-list>
        <ion-item v-for="d in dataCuaca" :key="d.time">
          <ion-label>
            <h3>{{ d.time }}</h3>
            <p>Suhu: {{ d.suhu }} &deg;C</p>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

import {
  IonPage, IonHeader, IonToolbar,
  IonTitle, IonContent, IonButton,
  IonList, IonItem, IonLabel
} from '@ionic/vue'

const dataCuaca = ref<any[]>([])
const error = ref('')

async function getWeather() {
  error.value = ''

  const url = 'https://api.open-meteo.com/v1/forecast?latitude=-6.2&longitude=106.8&hourly=temperature_2m'

  try {
    const res = await fetch(url)
    const json = await res.json()

    dataCuaca.value = json.hourly.time.slice(0, 10).map(
      (time: string, i: number) => ({
        time: time,
        suhu: json.hourly.temperature_2m[i]
      })
    )
  } catch {
    error.value = 'Gagal mengambil data cuaca.'
  }
}

onMounted(getWeather)
</script>