<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="primary">
        <ion-title>Cryptocurrency</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <div class="screen">
        <div class="actions">
          <ion-button @click="fetchData" :disabled="isLoading">
            {{ isLoading ? 'Loading...' : 'Refresh' }}
          </ion-button>
        </div>

        <p v-if="errorMessage" class="message error">{{ errorMessage }}</p>
        <p v-else-if="!isLoading && !coins.length" class="message">
          Belum ada data. Klik Refresh untuk mengambil daftar koin.
        </p>

        <div v-if="coins.length" class="list">
          <article v-for="item in coins" :key="item.id" class="row">
            <div class="column rank">
              <span class="label">Rank</span>
              <span class="value">{{ item.rank }}</span>
            </div>
            <div class="column identity">
              <span class="label name">{{ item.name }}</span>
              <span class="value symbol">{{ item.symbol }}</span>
            </div>
            <div class="column price">
              <span class="label">USD</span>
              <span class="value">{{ item.price_usd }}</span>
            </div>
          </article>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonButton, IonContent, IonHeader, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
import { defineComponent } from 'vue';

import EndPointAccess from '@/services/EndPointAccess';

interface Coin {
  id: string;
  rank: number;
  name: string;
  symbol: string;
  price_usd: string;
}

interface TickersResponse {
  data?: Coin[];
}

const URL_TICKERS = 'https://api.coinlore.net/api/tickers/';

export default defineComponent({
  name: 'HomePage',
  components: {
    IonButton,
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
  },
  data() {
    return {
      coins: [] as Coin[],
      isLoading: false,
      errorMessage: '',
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      this.isLoading = true;
      this.errorMessage = '';
      const endpoint = new EndPointAccess(URL_TICKERS);
      endpoint
        .getRes()
        .then((response: { data: TickersResponse }) => {
          this.coins = response.data.data ?? [];
          if (!this.coins.length) {
            this.errorMessage = 'API tidak mengembalikan daftar cryptocurrency.';
          }
        })
        .catch(() => {
          this.errorMessage = 'Gagal mengambil data. Periksa koneksi lalu klik Refresh.';
        })
        .finally(() => {
          this.isLoading = false;
        });
    },
  },
});
</script>

<style scoped>
.screen {
  max-width: 420px;
  margin: 0 auto;
  padding-bottom: 24px;
}

.actions {
  display: flex;
  justify-content: center;
  padding: 16px 0 8px;
}

.message {
  margin: 0 16px 12px;
  text-align: center;
  color: var(--ion-color-medium);
}

.error {
  color: var(--ion-color-danger);
}

.list {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 0 10px 12px;
}

.row {
  display: grid;
  grid-template-columns: 64px minmax(0, 1fr) auto;
  column-gap: 8px;
  align-items: center;
  background: #fff4c2;
  border-radius: 4px;
  padding: 10px 12px;
}

.column {
  min-width: 0;
}

.label {
  display: block;
  font-size: 12px;
  line-height: 1.2;
  color: #6b6252;
}

.value {
  display: block;
  margin-top: 2px;
  font-size: 20px;
  font-weight: 700;
  line-height: 1.2;
  color: #1f1a12;
}

.name {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.symbol {
  letter-spacing: 0.02em;
}

.price {
  text-align: right;
}

.price .value {
  font-size: 16px;
  font-variant-numeric: tabular-nums;
}
</style>
