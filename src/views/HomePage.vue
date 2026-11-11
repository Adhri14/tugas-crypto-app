<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Crypto App</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-refresher slot="fixed" @ionRefresh="refresh($event)">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>

      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Inbox</ion-title>
        </ion-toolbar>
      </ion-header>

      <div v-if="isLoading" class="spinner-container">
        <ion-spinner name="dots"></ion-spinner>
        <h2>Loading</h2>
      </div>
      <ion-list v-else>
        <CriptyListItem v-for="item in data" :key="item.id" :item="item" />
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonContent,
  IonHeader,
  IonList,
  IonPage,
  IonRefresher,
  IonRefresherContent,
  IonTitle,
  IonToolbar,
  IonSpinner,
} from '@ionic/vue';
import { onMounted, ref } from 'vue';
import { Cripto } from '@/data/crypto';
import CriptyListItem from '@/components/CriptyListItem.vue';

const data = ref<Cripto[]>([]);
const isLoading = ref<boolean>(true);

const getData = async () => {
  try {
    const url = 'https://api.coinlore.net/api/tickers/';
    const response = await fetch(url);
    const json = await response.json();
    data.value = json.data;
  } catch (error) {
    console.error('Error fetching data:', error);
  } finally {
    isLoading.value = false;
  }
}

const refresh = (ev: CustomEvent) => {
  isLoading.value = true;
  setTimeout(() => {
    ev.detail.complete();
    getData();
  }, 3000);
};

onMounted(() => {
  getData();
});
</script>

<style scoped>
.spinner-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
}
</style>
