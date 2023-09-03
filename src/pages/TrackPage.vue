<template>
  <q-page style="min-width: 100vw; height: max-content;" class="flex bg-btc flex-center text-white">
    <div style="max-width: 80vw; position: relative;" class="flex-center full-width q-pt-lg q-pb-md flex">
      <div style="margin-top: -30px; height: 15vh;"
        class="q-py-lg full-width justify-start column flex items-start q-px-xl q-py-md ">
        <div class="text-h3 col q-pt-lg q-pb-md flex text-bold">
          Ordinals Overall Stats
        </div>
        <div class="text-h6 col flex q-mt-sm  text-grey-5 text-bold">
          Wallet: {{ this.walletInserted && this.walletInserted }}
        </div>
      </div>
      <!-- GRAPHS STATS -->
      <div style="height: fit-content;" class="full-width q-py-lg row justify-between items-center q-gutter-x-md">
        <div style="width: 70%; height: 100%;" class="col justify-center flex items-center">
          <div style="border-radius: 3px;" class="full-width items-center q-py-md q-px-md justify-center full-height">
            <column-chart class="justify-center items-center flex q-mr-md" />
          </div>
        </div>
        <div style="width: 25%; height: 100%;" class="col column justify-center q-gutter-y-md flex  items-center">
          <div style="height: 50%; width: 100%; border: solid 0.5px #f7941a63; border-radius: 4px;"
            class="col flex column">
            <div style="border-bottom: solid 0.5px #f7941a63;"
              class="flex q-px-md q-py-md justify-start items-center full-width">
              <div class="text-h6 text-bold text-white">
                Performance.
              </div>
            </div>
            <div class="full-width q-pt-xl q-pb-md q-px-md justify-center flex-center items-center">
              <div v-if="!loadingData" class="justify-around flex-center items-center flex row full-width">
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold text-h6">
                    Total Minted
                  </div>
                  <div class="text-bold flex row items-center text-h6">
                    <div>
                      {{ parseFloat(this.overallMint).toFixed(7) }}
                    </div>
                    <q-icon class="text-bold" color="orange-5" size="xs" name="currency_bitcoin"></q-icon>
                  </div>
                </div>
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold text-h6">
                    Total Bought
                  </div>
                  <div class="text-bold flex row items-center text-h6">
                    <div>
                      {{ parseFloat(this.overallBuy).toFixed(7) }}
                    </div>
                    <q-icon class="text-bold" color="orange-5" size="xs" name="currency_bitcoin"></q-icon>
                  </div>

                </div>
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold text-h6">
                    Total Sold
                  </div>
                  <div class="text-bold flex row items-center text-h6">
                    <div>
                      {{ parseFloat(this.overallSell).toFixed(7) }}
                    </div>
                    <q-icon class="text-bold" color="orange-5" size="xs" name="currency_bitcoin"></q-icon>
                  </div>

                </div>
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold text-h6">
                    Total Count
                  </div>
                  <div class="text-bold text-h6">
                    {{ this.overallCount }}
                  </div>
                </div>
              </div>
              <div class="full-width flex justify-center items-center q-py-md" v-else>
                <q-spinner size="lg" color="orange"></q-spinner>
              </div>
            </div>
          </div>
          <div style="height: 50%; width: 100%; border: solid 0.5px #f7941a63; border-radius: 4px;"
            class="col flex column">
            <div style="border-bottom: solid 0.5px #f7941a63;"
              class="flex q-px-md q-py-md justify-start items-center full-width">
              <div class="text-h6 text-bold text-white">
                Holdings.
              </div>
            </div>
            <div class="full-width q-pt-xl q-pb-md q-px-md justify-center flex-center items-center">
              <div v-if="!loadingData" class="justify-around flex-center items-center flex row full-width">
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold text-h6">
                    ***
                  </div>
                  <div class="text-bold text-h6">
                    ***
                  </div>
                </div>
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold text-h6">
                    ***
                  </div>
                  <div class="text-bold text-h6">
                    ***
                  </div>

                </div>
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold text-h6">
                    ***
                  </div>
                  <div class="text-bold text-h6">
                    ***
                  </div>

                </div>
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold text-h6">
                    ***
                  </div>
                  <div class="text-bold text-h6">
                    ***
                  </div>
                </div>
              </div>
              <div class="full-width flex justify-center items-center q-py-md" v-else>
                <q-spinner size="lg" color="orange"></q-spinner>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- GRID COLLECTIONS -->
      <div v-if="!loadingData" style="height: fit-content;"
        class="full-width q-py-lg row justify-between items-center q-gutter-x-md">
        <div style="width: 100%; height: 100%;" class="col column justify-center q-gutter-y-md flex  items-center">
          <div style="height: 100%; width: 100%; border: solid 0.5px #f7941a63; border-radius: 4px;"
            class="col flex column">
            <div style="border-bottom: solid 0.5px #f7941a63;"
              class="flex q-px-md row q-gutter-x-xs q-py-md justify-start items-center full-width">
              <div class="">
                <q-btn @click="sectionGrid = 'assets'"
                  :class="sectionGrid == 'assets' ? 'text-h6 text-bold text-orange' : 'text-h6 text-bold'"
                  :style="sectionGrid == 'assets' ? 'border-bottom: solid 0.5px #f7941a63; border-radius: 2px;' : 'border-radius: 9px'"
                  flat label="Traded Assets" no-caps no-wrap dense>
                </q-btn>
              </div>
              <div class="">
                <q-btn @click="sectionGrid = 'activities'"
                  :class="sectionGrid == 'activities' ? 'text-h6 text-bold text-orange' : 'text-h6 text-bold'"
                  :style="sectionGrid == 'activities' ? 'border-bottom: solid 0.5px #f7941a63; border-radius: 2px;' : 'border-radius: 9px'"
                  flat label="Activities" no-caps no-wrap dense>
                </q-btn>
              </div>
            </div>
            <div style="height: 80vh; overflow-y: scroll;" v-if="sectionGrid == 'assets'"
              class="full-width q-pt-xl q-pb-md q-px-md justify-center flex-center items-center">
              <div style="height: max-content;" class="justify-center items-center flex row full-width">
                <div @click="openCollectionInfo(collection)"
                  class="flex q-mx-md cursor-pointer hover-effect q-my-md flex-wrap column items-center"
                  v-for="(collection, index) in dataCollections" :key="index">
                  <q-img :src="collection.imageURI"
                    style="max-width: 200px; max-height: 200px; height: 200px; width: 200px;" class="">

                  </q-img>
                  <div class="" style="max-width: 200px; width: 200px; height: 140px;border-radius: 2px;">
                    <div class="full-width q-py-sm column flex">
                      <div class="q-px-md text-subtitle2 text-bold">
                        {{ collection.name }}
                      </div>
                      <div class="q-px-md q-py-md full-width justify-between q-px-sm items-center row ">
                        <div class="column flex q-gutter-y-sm">
                          <div>
                            Supply
                          </div>
                          <div class="text-bold">
                            {{ collection.supply }}
                          </div>
                        </div>
                        <div class="column flex q-gutter-y-sm">
                          <div>
                            Interactions
                          </div>
                          <div class="text-bold justify-end items-right flex">
                            {{ collection.event.length }}
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="full-width q-pt-xl q-pb-md q-px-md justify-center flex-center items-center "
              style="height: 80vh; overflow-y: scroll" v-if="sectionGrid == 'activities'">
              <div style="height: max-content;" class="flex full-width justify-center q-px-sm column">
                <div v-for="(trade, index) in dataTrades" :key="index">
                  <div style="border: solid 0.5px orange; border-radius: 3px; height: 80px;"
                    class="full-width arrows-action cursor-pointer flex q-my-xs items-center justify-center">
                    <div class="full-width justify-between row items-center q-px-sm">
                      <div class="justify-start text-bold flex ">
                        {{ trade.name }}
                      </div>
                      <div class="justify-end items-right text-right column flex ">
                        <div class="flex row items-center q-gutter-x-xs text-right items-end justify-end"
                          v-if="trade.buy.length > 0">
                          <div>
                            <span class="text-bold text-cyan-2">Bought</span> for: <span class="text-bold">
                              {{ trade.buy[0].price }}
                            </span>
                          </div>
                          <div>
                            <q-icon name="currency_bitcoin" color="orange" size="0.77rem"></q-icon>
                          </div>
                        </div>
                        <div class="flex row items-center q-gutter-x-xs text-right items-end justify-end"
                          v-if="trade.mint.length > 0">
                          <div>
                            <span class="text-bold text-cyan-2">Minted</span> for: <span class="text-bold">
                              {{ trade.mint[0].price }}
                            </span>
                          </div>
                          <div>
                            <q-icon name="currency_bitcoin" color="orange" size="0.77rem"></q-icon>
                          </div>
                        </div>
                        <div class="flex row items-center q-gutter-x-xs text-right items-end justify-end"
                          v-if="trade.sell.length > 0">
                          <div>
                            <span class="text-bold text-orange-2">+ Sold</span> for: <span class="text-bold">
                              {{ trade.sell[0].price }}
                            </span>
                          </div>
                          <div>
                            <q-icon name="currency_bitcoin" color="orange" size="0.77rem"></q-icon>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

              </div>
            </div>
            <div>

            </div>
          </div>
        </div>
      </div>



    </div>
    <div v-if="loadingData" style="top: 0; right: 0; left: 0; bottom: 0; height: 100vh; position: fixed; opacity: 0.8;"
      class="flex row q-gutter-x-md fixed bg-btc flex-center items-center justify-center full-width">
      <div v-for="(text, index) in this.loadingTexts" v-show="index === currentTextIndex" :key="index"
        class="text-orange-5 text-bold text-h6 transition-text" :class="{ 'transition-out': index !== currentTextIndex }">
        {{ text }}
      </div>
      <div>
        <q-spinner size="lg" color="orange"></q-spinner>
      </div>
    </div>


    <q-dialog v-model="singleCollectionDialog">
      <q-card style="min-width: 65vw; min-height: fit-content;" class="bg-btc q-pb-md items-center text-white">
        <q-card-section>
          <div style="border-bottom: orange 0.5px solid; width: 100%;"
            class="flex q-py-lg q-px-md row justfy-between items-center">
            <div class="col row items-center q-gutter-x-md text-bold text-h6 justify-start flex">
              <div>
                <q-img style="width: 55px;" :src="collectionToSee.imageURI"></q-img>
              </div>
              <div>
                {{ collectionToSee.name }}
              </div>
            </div>
            <div class="col flex q-gutter-x-md row justify-end">
              <div v-if="this.collectionToSee.discordLink != ''">
                <q-btn :href="this.collectionToSee.discordLink" target="_blank" round icon="fab fa-discord"
                  color="orange-5" dense flat size="sm">
                  <q-tooltip>
                    Discord
                  </q-tooltip>
                </q-btn>
              </div>
              <div v-if="this.collectionToSee.twitterLink != ''">
                <q-btn :href="this.collectionToSee.twitterLink" target="_blank" round icon="fab fa-twitter"
                  color="orange-5" dense flat size="sm">
                  <q-tooltip>
                    Twitter
                  </q-tooltip>
                </q-btn>
              </div>
              <div v-if="this.collectionToSee.websiteLink != ''">
                <q-btn :href="this.collectionToSee.websiteLink" target="_blank" round icon="language" color="orange-5"
                  dense flat size="sm">
                  <q-tooltip>
                    Project website
                  </q-tooltip>
                </q-btn>
              </div>
              <div>
                <q-btn round icon="fas fa-info" color="orange-5" dense flat size="sm">
                  <q-tooltip>
                    About Collection
                  </q-tooltip>
                </q-btn>
              </div>
            </div>
          </div>
          <div class="q-pt-xl q-pb-md full-width q-px-lg flex items-center justify-center">
            <div class="justify-between items-center flex full-width q-py-md">
              <div class="text-h5 text-bold">
                Activities
              </div>
              <div class="flex justify-end items-center ">
                <div v-if="collectionToSee.symbol"
                  style="border: 0.5px solid rgba(255, 166, 0, 0.377); border-radius: 3px; "
                  class="flex q-pa-md q-px-lg column text-center q-gutter-y-sm">
                  <div style="font-size: 1.5rem; font-weight: 1000;">
                    {{ collectionToSee.symbol }}
                  </div>
                  <div>
                    Symbol
                  </div>
                </div>
                <div v-if="collectionToSee.event"
                  style="border: 0.5px solid rgba(255, 166, 0, 0.377); border-radius: 3px; "
                  class="flex q-pa-md text-center column q-gutter-y-sm">
                  <div style="font-size: 1.5rem; font-weight: 1000;">
                    {{ collectionToSee.event.length }}
                  </div>
                  <div>
                    Your Interactions
                  </div>
                </div>
                <div v-if="collectionToSee.supply"
                  style="border: 0.5px solid rgba(255, 166, 0, 0.377); border-radius: 3px; "
                  class="flex q-pa-md column text-center q-gutter-y-sm">
                  <div style="font-size: 1.5rem; font-weight: 1000;">
                    {{ collectionToSee.supply }}
                  </div>
                  <div>
                    Total Supply
                  </div>
                </div>
              </div>
            </div>
            <div style="border: solid 0.5px orange; height: 40vh; overflow-y: scroll;"
              class="full-width q-px-sm items-center">
              <div style="height: max-content;" class="full-width ">
                <div v-for="(event, index) in collectionToSee.event" :key="index">
                  <div style="border-bottom: solid 0.5px orange; height: 100px;"
                    class="row flex full-width items-center justify-center">
                    <div class="flex q-px-sm full-width justify-between items-center row">
                      <div class="justify-start row q-gutter-x-sm flex items-center">
                        <div>
                          <q-img
                            v-if="event.token.contentType != 'text/html;charset=utf-8' && event.token.contentType != 'text/html'"
                            :src="`https://ordinals.com/content/${event.tokenId}`"
                            style="width: 60px; height: 60px;"></q-img>
                          <iframe
                            v-if="event.token.contentType == 'text/html;charset=utf-8' || event.token.contentType == 'text/html'"
                            :src="`https://ordinals.com/content/${event.tokenId}`" width="60" height="60"></iframe>
                        </div>
                        <div class="column flex q-gutter-y-sm">
                          <div>
                            {{ event.token.meta ? event.token.meta.name : '' }}
                          </div>
                          <div>
                            <span style="font-size: 0.75rem;"> Inscription: </span> <span class="text-bold">{{
                              event.token.inscriptionNumber }}</span>
                          </div>
                        </div>
                      </div>
                      <div :class="`justify-end q-px-sm column flex items-end text-right q-gutter-y-sm`">
                        <div
                          :class="`
                          text-bold text-right q-px-sm items-end flex ${event.kind == 'create' || event.kind == 'mint' ? 'text-blue-2' : ''} ${event.kind == 'sell' ? 'text-orange-2' : ''} ${event.kind == 'recive' ? 'text-cyan-5' : ''}  ${event.kind == 'buy' ? 'text-cyan-2' : ''} ${event.kind == 'send' ? 'text-cyan-5' : ''}`">
                          {{ event.kind == 'create' ? 'MINTED' : '' }}
                          {{ event.kind == 'mint' ? 'MINTED' : '' }}
                          {{ event.kind == 'recive' ? 'RECIVE' : '' }}
                          {{ event.kind == 'sell' ? '+ SOLD' : '' }}
                          {{ event.kind == 'buy' ? 'BOUGHT' : '' }}
                          {{ event.kind == 'send' ? 'SEND' : '' }}
                        </div>
                        <div v-if="event.listedPrice > 0" class="row flex items-center q-gutter-x-xs">
                          <div>
                            {{ event.listedPrice ? event.listedPrice > 0 ? (parseFloat(event.listedPrice) /
                              100000000).toFixed(6) : '' : '' }}
                          </div>
                          <q-icon color="orange" name="currency_bitcoin" dense size="0.7rem"></q-icon>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import { defineAsyncComponent, defineComponent } from 'vue'
import axios from 'axios'
// import Chart from 'src/components/charts/line/ApexLine.vue'
const ApexLine = defineAsyncComponent(() =>
  import('src/components/charts/line/ApexLine.vue')
)
const ColumnChart = defineAsyncComponent(() =>
  import('src/components/charts/column/ColumnChart.vue')
)

export default defineComponent({
  name: 'TrackPage',
  data() {
    return {
      tutorials: false,
      loadingData: true,
      loadingTexts: ['Scrapping Sales', 'Scrapping Mints', 'Analyzing Data', 'Tracking Wallet', 'Searching Purchases', 'Calculating...', 'Scrapping Real Time Data...'],
      currentTextIndex: 0,
      dataTracked: [],
      dataTrades: [],
      dataCollections: [],
      overallBuy: null,
      overallMint: null,
      overallSell: null,
      overallCount: null,
      singleCollectionDialog: false,
      collectionToSee: [],
      sectionGrid: 'assets',
      walletInserted: this.$route.params.id.toString().slice(0, 6) + '...' + this.$route.params.id.toString().slice(-6),
    }
  },
  components: {
    ApexLine,
    ColumnChart
  },
  methods: {
    openCollectionInfo(collection) {
      this.singleCollectionDialog = true
      this.collectionToSee = collection
    },
    async getTrackedData() {
      const wallets = this.$route.params.id
      try {
        const response = await axios.get(`https://api.ordinalstracker.io/track/${wallets}`, {
          headers: {
            'Content-Type': 'application/json'
          }
        })
        this.dataTracked = response.data['ordinals-track']
        this.overallBuy = response.data['ordinals-track'].buy
        this.overallSell = response.data['ordinals-track'].sell
        this.overallMint = response.data['ordinals-track'].mint
        this.overallCount = response.data['ordinals-track'].count
        this.dataTrades = response.data['ordinals-track'].trades
        this.dataCollections = response.data['ordinals-track'].collections
        this.loadingData = false
        console.log('dataTracked', this.dataTracked)
        console.log('overallBuy', this.overallBuy)
        console.log('overallSell', this.overallSell)
        console.log('overallMint', this.overallMint)
        console.log('overallCount', this.overallCount)
        console.log('dataTrades', this.dataTrades)
        console.log('dataCollections', this.dataCollections)
      } catch (e) {
        console.error(e)
      }
    },
    changeText() {
      this.currentTextIndex = (this.currentTextIndex + 1) % this.loadingTexts.length;
    }
  },
  mounted() {
    this.getTrackedData()
    setInterval(this.changeText, 2500);

  }
})
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=VT323&display=swap');

* {
  font-family: 'VT323', monospace;
  margin: 0;
  padding: 0;
}

.bg-btc {
  background-color: #252424;
}

/* Estilo padrão */
.hover-effect {
  transition: transform 0.3s;
}

.arrows-action {
  transition: transform 0.3s;
}

.arrows-action:hover {
  transition: transform 0.3s;
  background-color: rgba(136, 122, 100, 0.439);
}

.hover-effect q-img {
  border: solid 0.6px #f7941a63;
}

.hover-effect>div {
  border-bottom: solid 2px orange;
  border-left: solid 2px orange;
  border-right: solid 2px orange;
  border-radius: 2px;
  transition: border-color 0.3s;
}

/* Estilo quando o mouse está sobre o elemento */
.hover-effect:hover {
  transform: translateZ(5px);
}

.hover-effect:hover q-img {
  border-color: black;
}

.hover-effect:hover>div {
  border-color: black;
}

.transition-text {
  transition: transform 0.5s, opacity 0.5s;
  transform: translateY(0);
  opacity: 1;
}

.transition-out {
  transform: translateY(-100%);
  opacity: 0;
}
</style>
