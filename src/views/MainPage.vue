<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <div class="toolbar-center">
          <ion-button size="small" :disabled="loading" @click="loadCoins"> Refresh </ion-button>
        </div>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <div v-if="loading" class="center">
        <ion-spinner name="crescent" />
      </div>

      <p v-if="error" class="center error">{{ error }}</p>

      <div v-for="coin in coins" :key="coin.id" class="coin-row">
        <div class="col rank">
          <span class="label">Rank</span>
          <span class="value">{{ coin.rank }}</span>
        </div>
        <div class="col name">
          <span class="label">{{ coin.name }}</span>
          <span class="value">{{ coin.symbol }}</span>
        </div>
        <div class="col price">
          <span class="label">USD</span>
          <span class="value small">{{ coin.price_usd }}</span>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { IonPage, IonHeader, IonToolbar, IonContent, IonButton, IonSpinner } from '@ionic/vue';

interface Coin {
  id: string;
  rank: number;
  name: string;
  symbol: string;
  price_usd: string;
}

interface TickersResponse {
  data: Coin[];
}

const API_URL = 'https://api.coinlore.net/api/tickers/';

const coins = ref<Coin[]>([]);
const loading = ref(false);
const error = ref('');

async function loadCoins() {
  loading.value = true;
  error.value = '';
  try {
    const response = await axios.get<TickersResponse>(API_URL);
    coins.value = response.data.data;
  } catch (e) {
    error.value = 'Failed to load data. Check your internet connection.';
  } finally {
    loading.value = false;
  }
}

onMounted(loadCoins);
</script>

<style scoped>
.toolbar-center {
  display: flex;
  justify-content: center;
  padding: 4px 0;
}

.center {
  text-align: center;
  padding: 16px;
}

.error {
  color: var(--ion-color-danger);
}

.coin-row {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px 12px;
  background: #fff1cc;
  border-bottom: 1px solid #e8d9a8;
}

.col {
  display: flex;
  flex-direction: column;
}

.rank {
  width: 15%;
}

.name {
  width: 45%;
}

.price {
  width: 40%;
}

.label {
  font-size: 12px;
  color: #333;
}

.value {
  font-size: 20px;
  color: #111;
}

.value.small {
  font-size: 18px;
}
</style>
