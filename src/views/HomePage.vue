<template>
  <ion-page>
    <ion-content :fullscreen="true" class="ion-padding">
      <div class="refresh-container">
        <ion-button color="primary" @click="fetchData">
          <ion-icon slot="start" :icon="refreshOutline"></ion-icon>
          Refresh
        </ion-button>
      </div>

      <div v-if="loading" class="loading">
        <ion-spinner name="crescent"></ion-spinner>
        <p>Loading...</p>
      </div>
      
      <div v-else class="crypto-list">
        <div v-for="coin in coins" :key="coin.id" class="crypto-item">
          <div class="col-rank">
            <div class="label">
              <ion-icon :icon="trophyOutline" class="label-icon"></ion-icon> Rank
            </div>
            <div class="value">{{ coin.rank }}</div>
          </div>
          <div class="col-name">
            <div class="label">
              <ion-icon :icon="diamondOutline" class="label-icon"></ion-icon> {{ coin.name }}
            </div>
            <div class="value">{{ coin.symbol }}</div>
          </div>
          <div class="col-price">
            <div class="label">
              <ion-icon :icon="cashOutline" class="label-icon"></ion-icon> USD
            </div>
            <div class="value">{{ coin.price_usd }}</div>
          </div>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { IonContent, IonPage, IonButton, IonIcon, IonSpinner } from '@ionic/vue';
import { refreshOutline, trophyOutline, diamondOutline, cashOutline } from 'ionicons/icons';

interface Coin {
  id: string;
  rank: number;
  name: string;
  symbol: string;
  price_usd: string;
}

const coins = ref<Coin[]>([]);
const loading = ref(false);

const fetchData = async () => {
  loading.value = true;
  try {
    const response = await fetch('https://api.coinlore.net/api/tickers/');
    const result = await response.json();
    // Mengambil hanya 10 data pertama menggunakan slice
    coins.value = result.data.slice(0, 10);
  } catch (error) {
    console.error('Error fetching data:', error);
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchData();
});
</script>

<style scoped>
.refresh-container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
  margin-bottom: 20px;
}

.crypto-list {
  background-color: #fdf2cd;
  border: 1px solid #d8b87c;
  border-radius: 4px;
  margin: 0 10px;
}

.crypto-item {
  display: flex;
  justify-content: space-between;
  padding: 12px 16px;
  border-bottom: 1px solid #d8b87c;
}

.crypto-item:last-child {
  border-bottom: none;
}

.col-rank {
  flex: 0 0 18%;
}

.col-name {
  flex: 1;
  padding-left: 10px;
}

.col-price {
  flex: 0 0 35%;
  text-align: left;
}

.label {
  font-size: 11px;
  color: #333;
  margin-bottom: 4px;
  display: flex;
  align-items: center;
  gap: 4px;
}

.label-icon {
  font-size: 14px;
  color: #92753c;
}

.value {
  font-size: 18px;
  font-weight: bold;
  color: #000;
}

.loading {
  text-align: center;
  margin-top: 50px;
  color: #666;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}
</style>
