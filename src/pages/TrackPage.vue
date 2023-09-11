<template>
  <q-page style="min-width: 100vw; max-width: 100vw; height: max-content;" class="flex bg-btc flex-center text-white">
    <div style="max-width: 80vw; position: relative;" class="flex-center full-width q-pt-lg q-pb-md flex">
      <div style="margin-top: -30px; height: 15vh;"
        class="q-py-lg gt-sm full-width justify-start column flex items-start q-px-xl q-py-md ">
        <div class="text-h3 col q-pt-lg q-pb-md flex text-bold">
          Ordinals Overall Stats
        </div>
        <div class="text-h6 col flex q-mt-lg  text-grey-5 text-bold">
          Wallet: {{ formattedWalletString }}
        </div>
      </div>
      <div style="margin-top: -30px; ;"
        class="q-py-lg q-px-lg lt-md full-width justify-start flex column items-start q-py-md ">
        <div class=" q-pt-lg  flex text-bold">
          Ordinals Overall Stats
        </div>
        <div class=" flex  text-grey-5 ">
          Wallet: {{ formattedWalletString }}
        </div>
      </div>
      <!-- GRAPHS STATS -->
      <!-- DESKTOP -->
      <div style="height: fit-content;" class="full-width gt-sm q-py-lg row justify-between items-center q-gutter-x-md">
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
            <div class="full-width q-pt-lg q-pb-md justify-center q-px-md flex-center items-center">
              <div v-if="!loadingData" class="justify-between flex-center items-center flex row full-width">
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold text-h6">
                    Total Minted
                  </div>
                  <div class="text-bold cursor-pointer flex row items-center text-h6">
                    <q-tooltip style="font-size: 0.99rem;" class="text-light">USD: ${{
                      parseFloat(this.overallMintUsd).toFixed(2) }}</q-tooltip>
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
                  <div class="text-bold cursor-pointer flex row items-center text-h6">
                    <q-tooltip style="font-size: 0.99rem;" class="text-light">USD: ${{ parseFloat(this.overallBuyUsd
                      .toFixed(2)) }}</q-tooltip>
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
                  <div class="text-bold cursor-pointer flex row items-center text-h6">
                    <q-tooltip style="font-size: 0.99rem;" class="text-light">USD: ${{
                      parseFloat(this.overallSellUsd).toFixed(2) }}</q-tooltip>
                    <div>
                      {{ parseFloat(this.overallSell).toFixed(7) }}
                    </div>
                    <q-icon class="text-bold" color="orange-5" size="xs" name="currency_bitcoin"></q-icon>
                  </div>

                </div>
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold text-h6">
                    PNL
                  </div>
                  <div class="text-bold cursor-pointer items-center flex text-h6">
                    <q-tooltip style="font-size: 0.99rem;" class="text-light">USD: ${{
                      parseFloat(this.overallPnlUsd).toFixed(2) }}</q-tooltip>
                    <div>
                      {{ parseFloat(this.overallPnl).toFixed(5) }}
                    </div>
                    <q-icon class="text-bold" color="orange-5" size="xs" name="currency_bitcoin"></q-icon>
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
              class="flex q-px-md q-py-md justify-between items-center full-width">
              <div class="flex justify-start">
                <div class="text-h6 text-bold text-white">Holdings.</div>
              </div>
              <div class="flex cursor-pointer row q-gutter-x-xs items-center justify-end">
                <q-tooltip style="font-size: 0.99rem;" class="text-bold">
                  USD: ${{ parseFloat(totalBalanceOrdinalsUsd).toFixed(2) }}
                </q-tooltip>
                <div class=" text-bold text-h6 text-white">{{ totalBalanceOrdinals }}</div>
                <q-icon name="currency_bitcoin" color="orange" size="xs"></q-icon>
              </div>
            </div>


            <q-table v-if="!loadingDataHoldings" v-model:pagination="pagination" hide-bottom :rows-per-page-options="[0]"
              dark :rows="dataHoldings" :columns="columns" row-key="name"
              style="height: 300px; overflow-y: scroll; border: none" class="q-pa-md bg-btc full-width">
              <template v-slot:body="props">
                <q-tr class="cursor-pointer" @click="openCollectionHolding(props.row.inscriptions, props.row)"
                  :props="props">
                  <q-td key="imageURI" :props="props">
                    <q-img style="width: 30px;" :src="props.row.imageURI" />
                  </q-td>
                  <q-td key="name" :props="props">{{ props.row.name }}</q-td>
                  <q-td key="inscriptions" class="flex items-center text-center justify-center" :props="props">
                    <div class="q-pr-md">
                      {{ parseFloat(props.row.inscriptions.length) }}
                    </div>
                  </q-td>
                  <q-td key="floorPrice" :props="props">
                    <div class="text-bold q-gutter-x-sm flex text-start flex-start items-center">
                      <q-icon name="currency_bitcoin" size="0.75rem" color="orange"></q-icon>
                      <div>{{ parseFloat(props.row.floorPrice).toFixed(5) }}</div>
                    </div>
                  </q-td>
                  <q-td key="holdingBalance" :props="props">
                    <div class="text-bold cursor-pointer q-gutter-x-sm flex text-start flex-start items-center">
                      <q-tooltip style="font-size: 0.99rem;" class="">
                        USD: ${{ parseFloat(props.row.usd_holdingBalance).toFixed(2) }}
                      </q-tooltip>
                      <q-icon name="currency_bitcoin" size="0.75rem" color="orange"></q-icon>
                      <div>{{ parseFloat(props.row.holdingBalance).toFixed(5) }}</div>
                    </div>
                  </q-td>
                </q-tr>
              </template>
            </q-table>
            <div class="full-width flex justify-center items-center q-py-md" v-else>
              <q-spinner size="lg" color="orange"></q-spinner>
            </div>
          </div>
        </div>
      </div>
      <!-- GRAPHS STATS -->
      <!-- MOBILE -->
      <div style="height: fit-content;"
        class="full-width lt-md q-py-lg column justify-between items-center q-gutter-y-md">
        <div style="width: 90%; height: 70%;" class=" flex-center justify-start flex items-start">
          <div style="border-radius: 3px;" class="full-width items-start flex-center q-py-md justify-start">
            <column-chart style="width: 100%" class="justify-center items-center flex" />
          </div>
        </div>
        <div style="width: 100vw; height: 90%;"
          class="col full-width column justify-center q-gutter-y-md flex  items-center">
          <div style="height: 50%; width: 100%; border: solid 0.5px #f7941a63; border-radius: 4px;"
            class="col flex column">
            <div style="border-bottom: solid 0.5px #f7941a63;"
              class="flex q-px-md q-py-md justify-start items-center full-width">
              <div class="text-h6 text-bold text-white">
                Performance.
              </div>
            </div>
            <div class="full-width q-pt-md q-pb-md q-px-md justify-center flex-center items-center">
              <div v-if="!loadingData" class="justify-around flex-center q-gutter-sm items-center flex row full-width">
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold ">
                    Minted
                  </div>
                  <div class="text-bold flex row items-center ">
                    <div>
                      {{ parseFloat(this.overallMint).toFixed(7) }}
                    </div>
                    <q-icon class="text-bold" color="orange-5" size="xs" name="currency_bitcoin"></q-icon>
                  </div>
                </div>
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold ">
                    Bought
                  </div>
                  <div class="text-bold flex row items-center ">
                    <div>
                      {{ parseFloat(this.overallBuy).toFixed(7) }}
                    </div>
                    <q-icon class="text-bold" color="orange-5" size="xs" name="currency_bitcoin"></q-icon>
                  </div>

                </div>
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold ">
                    Sold
                  </div>
                  <div class="text-bold flex row items-center ">
                    <div>
                      {{ parseFloat(this.overallSell).toFixed(7) }}
                    </div>
                    <q-icon class="text-bold" color="orange-5" size="xs" name="currency_bitcoin"></q-icon>
                  </div>

                </div>
                <div class="flex column q-gutter-y-sm items-center">
                  <div class="text-bold">
                    PNL
                  </div>
                  <div class="text-bold items-center flex">
                    <div>
                      {{ parseFloat(this.overallPnl).toFixed(5) }}
                    </div>
                    <q-icon class="text-bold" color="orange-5" size="xs" name="currency_bitcoin"></q-icon>
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
              class="flex q-px-md q-py-md justify-between items-center full-width">
              <div class="flex justify-start">
                <div class="text-h6 text-bold text-white">Holdings.</div>
              </div>
              <div class="flex row q-gutter-x-xs items-center justify-end">
                <div class=" text-bold text-h6 text-white">{{ totalBalanceOrdinals }}</div>
                <q-icon name="currency_bitcoin" color="orange" size="xs"></q-icon>
              </div>
            </div>


            <q-table v-if="!loadingDataHoldings" v-model:pagination="pagination" hide-bottom :rows-per-page-options="[0]"
              dark :rows="dataHoldings" :columns="columns" row-key="name"
              style="height: 300px; overflow-y: scroll; border: none" class="q-pa-md bg-btc full-width">
              <template v-slot:body="props">
                <q-tr class="cursor-pointer" @click="openCollectionHolding(props.row.inscriptions, props.row)"
                  :props="props">
                  <q-td key="imageURI" :props="props">
                    <q-img style="width: 30px;" :src="props.row.imageURI" />
                  </q-td>
                  <q-td key="name" :props="props">{{ props.row.name }}</q-td>
                  <q-td key="inscriptions" class="flex items-center text-center justify-center" :props="props">
                    <div class="q-pr-md">
                      {{ parseFloat(props.row.inscriptions.length) }}
                    </div>
                  </q-td>
                  <q-td key="floorPrice" :props="props">
                    <div class="text-bold q-gutter-x-sm flex text-start flex-start items-center">
                      <div>{{ parseFloat(props.row.floorPrice).toFixed(5) }}</div>
                    </div>
                  </q-td>
                  <q-td key="holdingBalance" :props="props">
                    <div class="text-bold q-gutter-x-sm flex text-start flex-start items-center">
                      <div>{{ parseFloat(props.row.holdingBalance).toFixed(5) }}</div>
                    </div>
                  </q-td>
                </q-tr>
              </template>
            </q-table>
            <div class="full-width flex justify-center items-center q-py-md" v-else>
              <q-spinner size="lg" color="orange"></q-spinner>
            </div>
          </div>
        </div>
      </div>
      <!-- GRID COLLECTIONS -->
      <div v-if="!loadingData" style="height: fit-content;"
        class="full-width gt-sm q-py-lg row justify-between items-center q-gutter-x-md">
        <div style="width: 100%; height: 100%;" class="col column justify-center q-gutter-y-md flex  items-center">
          <div style="height: 100%; width: 100%; border: solid 0.5px #f7941a63; border-radius: 4px;"
            class="col flex column">
            <div style="border-bottom: solid 0.5px #f7941a63;"
              class="flex q-px-md row q-gutter-x-xs q-py-md justify-start items-center full-width">
              <div class="">
                <q-btn @click="sectionGrid = 'assets'"
                  :class="sectionGrid == 'assets' ? 'text-h6 text-bold text-orange' : 'text-h6 text-bold'"
                  :style="sectionGrid == 'assets' ? 'border-bottom: solid 0.5px #f7941a63; border-radius: 2px;' : 'border-radius: 9px'"
                  flat label="Traded Collections" no-caps no-wrap dense>
                </q-btn>
              </div>
              <div class="">
                <q-btn @click="sectionGrid = 'activities'"
                  :class="sectionGrid == 'activities' ? 'text-h6 text-bold text-orange' : 'text-h6 text-bold'"
                  :style="sectionGrid == 'activities' ? 'border-bottom: solid 0.5px #f7941a63; border-radius: 2px;' : 'border-radius: 9px'"
                  flat label="Activities" no-caps no-wrap dense>
                </q-btn>
              </div>
              <div v-if="sectionGrid == 'activities'" class="flex col justify-end items-center">
                <q-btn label=".CSV" dense icon="download" flat color="orange" size="md" @click="downloadCSV" no-caps
                  no-wrap>
                </q-btn>
              </div>
            </div>
            <div style="height: 85vh; overflow-y: scroll;" v-if="sectionGrid == 'assets'"
              class="full-width q-pt-xl q-pb-md q-px-md">
              <div style="height: max-content;" class="justify-start flex-wrap items-start q-gutter-md flex full-width">
                <div style="width: 13%;" @click="openCollectionInfo(collection)"
                  class="flex cursor-pointer hover-effect q-my-md column" v-for="(collection, index) in dataCollections"
                  :key="index">
                  <q-img :src="collection.imageURI"
                    style="max-width: 100%; max-height: 200px; height: 200px; width: 200px;" class="">

                  </q-img>
                  <div class="" style="max-width: 100%; width: 200px; height: 140px;border-radius: 2px;">
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
            <q-table v-model:pagination="pagination" :rows-per-page-options="[0]" dark
              class="text-white custom-table text-bold bg-btc" v-if="sectionGrid == 'activities'" style="height: 400px"
              flat :rows="dataTrades" :columns="tableColumns" color="black" row-key="index" hide-bottom>
              <template v-slot:body="props">
                <q-tr :props="props">
                  <q-td key="name" :props="props">
                    <div class="row items-center q-gutter-x-sm">
                      <q-img
                        v-if="props.row.inscription_contentType != 'text/html;charset=utf-8' && props.row.inscription_contentType != 'text/html'"
                        :src="`https://ordinals.com/content/${props.row.id}`" style="width: 30px;"></q-img>
                      <iframe
                        v-if="props.row.inscription_contentType == 'text/html;charset=utf-8' || props.row.inscription_contentType == 'text/html'"
                        :src="`https://ordinals.com/content/${props.row.id}`" width="30" height="30"></iframe>
                      <div>
                        <span @click="openInNewTab(`https://magiceden.io/ordinals/item-details/${props.row.id}`)"
                          class="cursor-pointer"> {{ props.row.name != '' ? props.row.name : props.row.collection
                          }}</span>

                      </div>
                    </div>
                  </q-td>
                  <q-td key="collection" :props="props">
                    <div class="row items-center q-gutter-x-sm">
                      <div>
                        <span @click="openInNewTab(`https://magiceden.io/ordinals/item-details/${props.row.id}`)"
                          class="cursor-pointer"> {{ props.row.collection
                          }}</span>

                      </div>
                    </div>
                  </q-td>
                  <q-td key="buy" :props="props">
                    <div v-if="props.row.buy.length > 0" class="flex row items-center q-gutter-x-xs">
                      <div>
                        <span @click="openInNewTab(`https://mempool.space/tx/${props.row.buy[0].tx}`)"
                          class="text-bold cursor-pointer text-red-5">{{
                            '- ' + props.row.buy[0].price }}
                          <q-tooltip class="text-bold text-h6 column flex q-gutter-y-sm">
                            <div style="font-size: 0.8rem;">
                              {{
                                convertTimestampToDate(props.row.buy[0].when)
                              }}
                            </div>
                            <div style="font-size: 0.9rem; font-weight: 300;" class="">
                              USD: ${{ parseFloat(props.row.buy[0].usd_price).toFixed(2) }}
                            </div>
                          </q-tooltip>
                        </span>
                      </div>
                      <q-icon name="currency_bitcoin" color="orange" size="0.77rem"></q-icon>
                    </div>
                  </q-td>
                  <q-td key="mint" :props="props">
                    <div v-if="props.row.mint.length > 0" class="flex row items-center q-gutter-x-xs">
                      <div>
                        <span @click="openInNewTab(`https://mempool.space/tx/${props.row.mint[0].tx}`)"
                          class="text-bold cursor-pointer text-cyan-5">{{
                            '- ' + props.row.mint[0].price }}
                          <q-tooltip class="text-bold text-h6 column flex q-gutter-y-sm">
                            <div style="font-size: 0.8rem;">
                              {{
                                convertTimestampToDate(props.row.mint[0].when)
                              }}
                            </div>
                            <div style="font-size: 0.9rem; font-weight: 300;" class="">
                              USD: ${{ parseFloat(props.row.mint[0].usd_price).toFixed(2) }}
                            </div>
                          </q-tooltip>

                        </span>
                      </div>
                      <q-icon name="currency_bitcoin" color="orange" size="0.77rem"></q-icon>
                    </div>
                  </q-td>
                  <q-td key="sell" :props="props">
                    <div v-if="props.row.sell.length > 0" class="flex row items-center q-gutter-x-xs">
                      <div>
                        <span @click="openInNewTab(`https://mempool.space/tx/${props.row.sell[0].tx}`)"
                          class="text-bold cursor-pointer text-green-5">{{
                            '+ ' + props.row.sell[0].price }}
                          <q-tooltip class="text-bold text-h6 column flex q-gutter-y-sm">
                            <div style="font-size: 0.8rem;">
                              {{
                                convertTimestampToDate(props.row.sell[0].when)
                              }}
                            </div>
                            <div style="font-size: 0.9rem; font-weight: 300;" class="">
                              USD: ${{ parseFloat(props.row.sell[0].usd_price).toFixed(2) }}
                            </div>
                          </q-tooltip>
                        </span>
                      </div>
                      <q-icon name="currency_bitcoin" color="orange" size="0.77rem"></q-icon>
                    </div>
                  </q-td>
                  <q-td key="pnl" :props="props">
                    <div class="flex row items-center q-gutter-x-xs">
                      <div>
                        <span v-if="getPNL(props.row).value !== null"
                          :class="getPNL(props.row).value >= 0 ? 'text-bold cursor-pointer text-green-5' : 'text-bold cursor-pointer text-red-5'">
                          {{ getPNL(props.row).value.toFixed(5) }}
                          <q-tooltip class="text-bold text-h6 column flex q-gutter-y-sm">
                            <div style="font-size: 0.8rem;">
                              {{
                                convertTimestampToDate(props.row.sell[0].when)
                              }}
                            </div>
                            <div style="font-size: 0.9rem; font-weight: 300;" class="">
                              USD: ${{ getPNLUsd(props.row).value.toFixed(2) }}
                            </div>
                          </q-tooltip>

                        </span>
                      </div>
                      <q-icon v-if="getPNL(props.row).value !== null" name="currency_bitcoin" color="orange"
                        size="0.77rem"></q-icon>
                      <div v-if="getPNL(props.row).percentage !== null">
                        <span :class="getPNL(props.row).value >= 0 ? 'text-green-5' : 'text-red-5'">
                          ({{ getPNL(props.row).percentage.toFixed(2) }}%)
                        </span>
                      </div>
                    </div>
                  </q-td>
                </q-tr>
              </template>
            </q-table>


            <div>

            </div>
          </div>
        </div>
      </div>
      <!-- GRID MOBILE -->
      <div v-if="!loadingData" style="height: fit-content; width: 100vw;"
        class="lt-md q-py-lg row justify-between items-center q-gutter-x-md">
        <div style="width: 100%; height: 100%;" class="col column justify-center q-gutter-y-md flex  items-center">
          <div style="height: 100%; width: 100%; border: solid 0.5px #f7941a63; border-radius: 4px;"
            class="col flex column">
            <div style="border-bottom: solid 0.5px #f7941a63;"
              class="flex q-px-md row q-gutter-x-xs q-py-md justify-start items-center full-width">
              <div class="">
                <q-btn @click="sectionGrid = 'assets'" :class="sectionGrid == 'assets' ? ' text-bold text-orange' : ''"
                  :style="sectionGrid == 'assets' ? 'border-bottom: solid 0.5px #f7941a63; border-radius: 2px;' : 'border-radius: 9px'"
                  flat label="Traded Collections" no-caps no-wrap dense>
                </q-btn>
              </div>
              <div class="">
                <q-btn @click="sectionGrid = 'activities'"
                  :class="sectionGrid == 'activities' ? 'text-bold text-orange' : ''"
                  :style="sectionGrid == 'activities' ? 'border-bottom: solid 0.5px #f7941a63; border-radius: 2px;' : 'border-radius: 9px'"
                  flat label="Activities" no-caps no-wrap dense>
                </q-btn>
              </div>
              <div v-if="sectionGrid == 'activities'" class="flex col justify-end items-center">
                <q-btn label=".CSV" dense icon="download" flat color="orange" size="md" @click="downloadCSV" no-caps
                  no-wrap>
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
            <q-table v-model:pagination="pagination" :rows-per-page-options="[0]" dark
              class="text-white full-width text-bold bg-btc" v-if="sectionGrid == 'activities'" style="height: 400px" flat
              :rows="dataTrades" :columns="tableColumns" color="black" row-key="index" hide-bottom>
              <template v-slot:body="props">
                <q-tr :props="props">
                  <q-td key="name" :props="props">
                    <div class="row items-center q-gutter-x-sm">
                      <div style="font-size: 0.77rem;">
                        <span @click="openInNewTab(`https://magiceden.io/ordinals/item-details/${props.row.id}`)"
                          class="cursor-pointer"> {{ props.row.name != '' ? props.row.name.length > 6 ?
                            props.row.name.slice(0, 6) + '...' : props.row.name : props.row.collection
                          }}</span>

                      </div>
                    </div>
                  </q-td>
                  <q-td key="collection" :props="props">
                    <div class="row items-center q-gutter-x-sm">
                      <div style="font-size: 0.77rem;">
                        <span @click="openInNewTab(`https://magiceden.io/ordinals/item-details/${props.row.id}`)"
                          class="cursor-pointer"> {{ props.row.collection.length > 8 ? props.row.collection.slice(0, 8) +
                            '...' : props.row.collection
                          }}</span>

                      </div>
                    </div>
                  </q-td>
                  <q-td key="buy" :props="props">
                    <div v-if="props.row.buy.length > 0" class="flex row items-center q-gutter-x-xs">
                      <div>
                        <span @click="openInNewTab(`https://mempool.space/tx/${props.row.buy[0].tx}`)"
                          class="text-bold cursor-pointer text-red-5">{{
                            '- ' + props.row.buy[0].price }}
                          <q-tooltip class="text-bold text-h6">{{ convertTimestampToDate(props.row.buy[0].when)
                          }}</q-tooltip>
                        </span>
                      </div>
                    </div>
                  </q-td>
                  <q-td key="mint" :props="props">
                    <div v-if="props.row.mint.length > 0" class="flex row items-center q-gutter-x-xs">
                      <div>
                        <span @click="openInNewTab(`https://mempool.space/tx/${props.row.mint[0].tx}`)"
                          class="text-bold cursor-pointer text-cyan-5">{{
                            '- ' + props.row.mint[0].price }}
                          <q-tooltip class="text-bold text-h6">{{ convertTimestampToDate(props.row.mint[0].when)
                          }}</q-tooltip>
                        </span>
                      </div>
                    </div>
                  </q-td>
                  <q-td key="sell" :props="props">
                    <div v-if="props.row.sell.length > 0" class="flex row items-center q-gutter-x-xs">
                      <div>
                        <span @click="openInNewTab(`https://mempool.space/tx/${props.row.sell[0].tx}`)"
                          class="text-bold cursor-pointer text-green-5">{{
                            '+ ' + props.row.sell[0].price }}
                          <q-tooltip class="text-bold text-h6">{{ convertTimestampToDate(props.row.sell[0].when)
                          }}</q-tooltip>
                        </span>
                      </div>
                    </div>
                  </q-td>
                  <q-td key="pnl" :props="props">
                    <div class="flex row items-center q-gutter-x-xs">
                      <div>
                        <span v-if="getPNL(props.row).value !== null"
                          :class="getPNL(props.row).value >= 0 ? 'text-bold cursor-pointer text-green-5' : 'text-bold cursor-pointer text-red-5'">
                          {{ getPNL(props.row).value.toFixed(5) }}
                          <q-tooltip class="text-bold text-h6">{{ convertTimestampToDate(props.row.sell[0]?.when)
                          }}</q-tooltip>
                        </span>
                      </div>
                      <div v-if="getPNL(props.row).percentage !== null">
                        <span :class="getPNL(props.row).value >= 0 ? 'text-green-5' : 'text-red-5'">
                          ({{ getPNL(props.row).percentage.toFixed(2) }}%)
                        </span>
                      </div>
                    </div>
                  </q-td>
                </q-tr>
              </template>
            </q-table>


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

    <q-dialog v-model="dialogHoldingCollection">
      <q-card style="width: 44vw; height: 44vh;" class="bg-dark">
        <q-card-section>
          <div class="justify-between items-center flex q-py-sm">
            <div class="justify-start text-white q-px-md q-py-md text-bold items-center flex ">
              {{ this.collectiontoSeeHoldedStats.name }}
            </div>
            <div class="justify-end column q-gutter-x-xs text-white q-px-md q-py-md text-bold items-center flex ">
              <div class="flex row items-center q-gutter-x-xs">
                <div class="text-bold">
                  {{ parseFloat(this.collectiontoSeeHoldedStats.holdingBalance) }}
                </div>
                <div>
                  <q-icon name="currency_bitcoin" size="0.75rem" color="orange"></q-icon>
                </div>
              </div>
              <div style="font-weight: 300;">
                Balance
              </div>
            </div>
          </div>
          <div style="height: 33vh; overflow-y: scroll;">
            <div style="height: max-content;"
              class="grid q-gutter-md full-width flex-wrap flex items-center justify-start full-width q-px-sm q-py-sm">
              <div @click="openInNewTab(`https://ord.io/${item.inscriptionNumber}`)"
                style="width: 21%; height: fit-content; border: solid 0.5px orange;"
                class="text-white grid-holding column flex q-gutter-y-none text-bold cursor-pointer items-start"
                v-for="(item, index) in this.collectiontoSeeHolded" :key="index">
                <q-img
                  v-if="item.contentType != 'text/html' && item.contentType != 'text/html;charset=utf-8' && item.contentType != 'text/plain;charset=utf-8'"
                  style="width: 100;" :src="item.contentURI"></q-img>
                <iframe
                  v-if="item.contentType == 'text/html;charset=utf-8' || item.contentType == 'text/html' || item.contentType == 'text/plain;charset=utf-8'"
                  :src="item.contentURI" style="width: 100%; height: 100%;" width="100" height="100"></iframe>
                <div class="items-start justify-start column flex q-py-xs q-px-sm"
                  style="width: 100; height: fit-content;">
                  <div>
                    Inscription
                  </div>
                  <div>
                    {{ item.inscriptionNumber }}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </q-card-section>
      </q-card>
    </q-dialog>

    <q-dialog v-model="singleCollectionDialog">
      <q-card style="min-width: 65vw; min-height: fit-content;" class="bg-btc q-pb-md items-center text-white">
        <q-card-section>
          <div style="border-bottom: orange 0.5px solid; width: 100%;"
            class="flex q-py-lg q-px-md row justfy-between items-center">
            <div class="col row items-center q-gutter-x-md text-bold text-h6 justify-start flex">
              <div class="gt-sm">
                <q-img style="width: 55px;" :src="collectionToSee.imageURI"></q-img>
              </div>
              <div>
                {{ collectionToSee.name }}
              </div>
            </div>
            <div class="col flex q-gutter-x-md row justify-end">
              <div>
                <q-btn target="_blank" :href="`https://magiceden.io/ordinals/marketplace/${collectionToSee.symbol}`" dense
                  round size="sm" flat>
                  <q-avatar size="xs" round>
                    <q-img src="https://pbs.twimg.com/profile_images/1668003671626350592/LvmVzo2C_400x400.jpg"></q-img>
                  </q-avatar>
                  <q-tooltip>
                    View on ME
                  </q-tooltip>
                </q-btn>
              </div>
              <div v-if="this.collectionToSee.discordLink != ''">
                <q-btn :href="this.collectionToSee.discordLink" target="_blank" round icon="fab fa-discord" color=""
                  style="color: #FE5A21;" dense flat size="sm">
                  <q-tooltip>
                    Discord
                  </q-tooltip>
                </q-btn>
              </div>
              <div v-if="this.collectionToSee.twitterLink != ''">
                <q-btn :href="this.collectionToSee.twitterLink" target="_blank" round icon="fab fa-twitter" color=""
                  style="color: #FE5A21;" dense flat size="sm">
                  <q-tooltip>
                    Twitter
                  </q-tooltip>
                </q-btn>
              </div>
              <div v-if="this.collectionToSee.websiteLink != ''">
                <q-btn :href="this.collectionToSee.websiteLink" target="_blank" round icon="language"
                  style="color: #FE5A21;" color="" dense flat size="sm">
                  <q-tooltip>
                    Project website
                  </q-tooltip>
                </q-btn>
              </div>

            </div>
          </div>
          <div class="q-pt-sm q-pb-md full-width q-px-sm flex items-center justify-center">
            <div class="items-center gt-sm flex full-width q-py-md">
              <div class="flex justify-between full-width q-gutter-md items-center ">
                <div class="justify-start flex gt-sm items-center ">
                  <div class="">
                    Activities
                  </div>
                </div>
                <div class="justify-end flex items-center ">
                  <div style="border: 0.5px solid orange; border-radius: 3px;" v-if="collectionToSee.floorPrice"
                    class="flex q-pa-md text-center column q-gutter-y-sm">
                    <div class="row flex items-center">
                      <div style="font-size: 0.8rem; font-weight: 1000;">
                        {{ collectionToSee.totalVolume }}
                      </div>
                      <q-icon name="currency_bitcoin" color="white" size="xs"></q-icon>
                    </div>
                    <div>
                      Total Volume
                    </div>
                  </div>
                  <div style="border: 0.5px solid orange; border-radius: 3px;" v-if="collectionToSee.floorPrice"
                    class="flex q-pa-md text-center column q-gutter-y-sm">
                    <div class="flex row items-center">
                      <div style="font-size: 0.8rem; font-weight: 1000;">
                        {{ collectionToSee.floorPrice }}
                      </div>
                      <q-icon name="currency_bitcoin" color="white" size="xs"></q-icon>

                    </div>
                    <div>
                      Floor Price
                    </div>
                  </div>
                  <div style="border: 0.5px solid orange; border-radius: 3px;" v-if="collectionToSee.supply"
                    class="flex q-pa-md column text-center q-gutter-y-sm">
                    <div style="font-size: 0.8rem; font-weight: 1000;">
                      {{ collectionToSee.supply }}
                    </div>
                    <div>
                      Total Supply
                    </div>
                  </div>
                  <div style="border: 0.5px solid orange; border-radius: 3px;" v-if="collectionToSee.event"
                    class="flex q-pa-md text-center column q-gutter-y-sm">
                    <div style="font-size: 0.8rem; font-weight: 1000;">
                      {{ collectionToSee.event.length }}
                    </div>
                    <div>
                      Interactions
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <div class="items-center lt-md flex full-width q-py-md">
              <div class="justify-between q-gutter-md flex items-center ">
                <div style="border: 0.5px solid orange; border-radius: 3px;" v-if="collectionToSee.floorPrice"
                  class="flex q-pa-md text-center column q-gutter-y-sm">
                  <div class="row flex items-center">
                    <div style="font-size: 0.8rem; font-weight: 1000;">
                      {{ collectionToSee.totalVolume }}
                    </div>
                    <q-icon name="currency_bitcoin" color="white" size="xs"></q-icon>
                  </div>
                  <div>
                    Total Volume
                  </div>
                </div>
                <div style="border: 0.5px solid orange; border-radius: 3px;" v-if="collectionToSee.floorPrice"
                  class="flex q-pa-md text-center column q-gutter-y-sm">
                  <div class="flex row items-center">
                    <div style="font-size: 0.8rem; font-weight: 1000;">
                      {{ collectionToSee.floorPrice }}
                    </div>
                    <q-icon name="currency_bitcoin" color="white" size="xs"></q-icon>

                  </div>
                  <div>
                    Floor Price
                  </div>
                </div>
                <div style="border: 0.5px solid orange; border-radius: 3px;" v-if="collectionToSee.supply"
                  class="flex q-pa-md column text-center q-gutter-y-sm">
                  <div style="font-size: 0.8rem; font-weight: 1000;">
                    {{ collectionToSee.supply }}
                  </div>
                  <div>
                    Total Supply
                  </div>
                </div>
                <div style="border: 0.5px solid orange; border-radius: 3px;" v-if="collectionToSee.event"
                  class="flex q-pa-md text-center column q-gutter-y-sm">
                  <div style="font-size: 0.8rem; font-weight: 1000;">
                    {{ collectionToSee.event.length }}
                  </div>
                  <div>
                    Interactions
                  </div>
                </div>
              </div>
            </div>


            <div style="border: solid 0.5px orange; height: 40vh; overflow-y: scroll;"
              class="full-width q-px-sm items-center">
              <div style="height: max-content;" class="full-width ">
                <div v-for="(event, index) in collectionToSee.event" :key="index">
                  <div style="border-bottom: solid 0.5px orange; height: 100px;"
                    class="row arrows-event cursor-pointer flex full-width items-center justify-center">
                    <q-tooltip class="text-h6">
                      {{ event.createdAt }}
                    </q-tooltip>
                    <div class="flex q-px-sm full-width justify-between items-center row">

                      <div class="justify-start row q-gutter-x-sm flex items-center">

                        <div>
                          <q-img
                            v-if="event.token.contentType != 'text/html;charset=utf-8' && event.token.contentType != 'text/html'"
                            :src="`https://ordinals.com/content/${event.tokenId}`"
                            style="width: 35px; height: 35px;"></q-img>
                          <iframe
                            v-if="event.token.contentType == 'text/html;charset=utf-8' || event.token.contentType == 'text/html'"
                            :src="`https://ordinals.com/content/${event.tokenId}`" width="35" height="35"></iframe>
                        </div>
                        <div class="column flex ">
                          <div class="lt-md">
                            {{ event.token.meta ? event.token.meta.name.length > 7 ? event.token.meta.name.slice(0, 7) +
                              '...' : event.token.meta.name : '' }}
                          </div>
                          <div class="gt-sm">
                            {{ event.token.meta ? event.token.meta.name : '' }}
                          </div>
                          <div>
                            <span style="font-size: 0.75rem;"> Inscription: </span> <span class="text-bold">{{
                              event.token.inscriptionNumber }}</span>
                          </div>
                        </div>
                      </div>
                      <div :class="`justify-end q-px-sm column flex items-end text-right q-gutter-y-sm`">

                        <div style="font-size: 0.72rem;"
                          :class="`
                          text-bold text-right q-px-sm items-end flex ${event.kind == 'create' || event.kind == 'Mint' ? 'text-blue-2' : ''} ${event.kind == 'Sell' ? 'text-orange-2' : ''} ${event.kind == 'Receive' ? 'text-cyan-5' : ''}  ${event.kind == 'Buy' ? 'text-cyan-2' : ''} ${event.kind == 'send' ? 'text-cyan-5' : ''}`">
                          {{ event.kind == 'create' ? 'MINTED' : '' }}
                          {{ event.kind == 'Mint' ? 'MINTED' : '' }}
                          {{ event.kind == 'Receive' ? 'RECEIVE' : '' }}
                          {{ event.kind == 'Sell' ? 'SOLD' : '' }}
                          {{ event.kind == 'Buy' ? 'BOUGHT' : '' }}
                          {{ event.kind == 'Send' ? 'SEND' : '' }}
                        </div>
                        <div v-if="event.listedPrice > 0" class="row flex items-center q-gutter-x-xs">
                          <div :class="event.kind == 'Sell' ? 'text-green-5' : 'text-red-5'">
                            {{ event.kind == 'Sell' ? '+ ' : '- ' }} {{ event.listedPrice ? event.listedPrice > 0 ?
                              (parseFloat(event.listedPrice) /
                                100000000).toFixed(6) : '' : '' }}
                          </div>
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

    <q-dialog v-model="dialogAboutCollection">
      <q-card style="width: fit-content; height: fit-content;" class="bg-dark q-pa-lg">
        <q-card-section>
          <div style="width: 80%;" class="flex justify-center text-h6 text-bold text-white">
            {{ collectionToSee.description }}
          </div>
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import { defineAsyncComponent, defineComponent, ref } from 'vue'
import axios from 'axios'
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
      tableColumns: [
        { name: 'name', required: true, label: 'Inscription', align: 'left', field: 'name' },
        { name: 'collection', required: true, label: 'Collection', align: 'left', field: 'collection' },
        { name: 'buy', required: true, label: 'Bought', align: 'left', field: 'buy' },
        { name: 'mint', required: true, label: 'Minted', align: 'left', field: 'mint' },
        { name: 'sell', required: true, label: 'Sold', align: 'left', field: 'sell' },
        { name: 'pnl', required: true, label: 'PNL', align: 'left', field: 'pnl' }
      ],
      columns: [
        { name: 'imageURI', label: 'Icon', align: 'left', field: row => row.imageURI },
        { name: 'name', label: 'Collection', align: 'left', field: row => row.name },
        { name: 'inscriptions', label: 'Amount', align: 'left', field: row => row.inscriptions.length },
        { name: 'floorPrice', label: 'Floor Price', align: 'left', field: row => row.floorPrice },
        { name: 'holdingBalance', label: 'Balance', align: 'left', field: row => row.holdingBalance },
      ],
      loadingData: true,
      loadingDataHoldings: true,
      loadingTexts: ['Scraping Sales', 'Scraping Mints', 'Analyzing Data', 'Tracking Wallet', 'Searching Purchases', 'Calculating...', 'Scraping Real Time Data...'],
      currentTextIndex: 0,
      dataTracked: [],
      dataTrades: [],
      dialogHoldingCollection: false,
      collectiontoSeeHolded: [],
      collectiontoSeeHoldedStats: [],
      totalBalanceOrdinals: null,
      totalBalanceOrdinalsUsd: null,
      dataCollections: [],
      dataHoldings: [],
      dataHoldings: [],
      overallBuy: null,
      overallMint: null,
      overallSell: null,
      overallCount: null,
      overallPnl: null,
      overallBuyUsd: null,
      overallMintUsd: null,
      overallSellUsd: null,
      overallPnlUsd: null,
      singleCollectionDialog: false,
      dialogAboutCollection: false,
      collectionToSee: [],
      sectionGrid: 'assets',
      walletInserted: this.$route.params.id
    }
  },
  computed: {
    formattedWalletString() {
      let walletsAdd = this.$route.params.id.split(',');

      if(walletsAdd.length === 1) {
        return walletsAdd[0].slice(0, 6) + '...' + walletsAdd[0].slice(-6);
      } else {
        return walletsAdd[0].slice(0, 6) + '...' + walletsAdd[0].slice(-6) + ' ...+' + (walletsAdd.length - 1);
      }
    }
  },
  setup() {
    pagination: ref({
      rowsPerPage: 0
    })
  },
  components: {
    ApexLine,
    ColumnChart
  },
  methods: {
    getPNL(row) {
      if (row.sell.length === 0) {
        return { value: null, percentage: null };
      }
      let initialPrice = 0;
      let finalPrice = parseFloat(row.sell[0]?.price || 0);

      if (row.mint.length > 0) {
        initialPrice = parseFloat(row.mint[0]?.price || 0);
      } else if (row.buy.length > 0) {
        initialPrice = parseFloat(row.buy[0]?.price || 0);
      }

      let value = finalPrice - initialPrice;
      let percentage = (initialPrice > 0) ? ((value / initialPrice) * 100) : null;

      return { value, percentage };
    },
    getPNLUsd(row) {
      if (row.sell.length === 0) {
        return { value: null, percentage: null };
      }
      let initialPrice = 0;
      let finalPrice = parseFloat(row.sell[0]?.usd_price || 0);

      if (row.mint.length > 0) {
        initialPrice = parseFloat(row.mint[0]?.usd_price || 0);
      } else if (row.buy.length > 0) {
        initialPrice = parseFloat(row.buy[0]?.usd_price || 0);
      }

      let value = finalPrice - initialPrice;
      let percentage = (initialPrice > 0) ? ((value / initialPrice) * 100) : null;

      return { value, percentage };
    },
    openCollectionHolding(collection, stats) {
      this.dialogHoldingCollection = true
      this.collectiontoSeeHolded = collection
      this.collectiontoSeeHoldedStats = stats
    },
    openInNewTab(url) {
      window.open(url, '_blank');
    },
    convertTimestampToDate(timestamp) {
      const date = new Date(timestamp * 1000);
      return date.toLocaleDateString();
    },
    downloadCSV() {
      const csvRows = [];

      const headers = [
        ...this.tableColumns.map((col) => col.label).filter((label) => label !== 'PNL'),
        'Inscription ID',
        'Buy Timestamp',
        'Sell Timestamp',
        'Buy TxID',
        'Sell TxID',
        'Minted Timestamp',
        'Minted TxID',
        'Mint Price (USD)',
        'Buy Price (USD)',
        'Sell Price (USD)',
        'PNL (%)',
        'pnl',
        'PNL (USD)',
      ];
      csvRows.push(headers.join(';'));


      this.dataTrades.forEach((row) => {
        const values = this.tableColumns.map((col) => {
          if (col.label === 'PNL') return;
          let val = row[col.name];
          if (Array.isArray(val) && val.length > 0) {
            val = parseFloat(val[0]?.price || 0).toFixed(5);
          } else {
            val = val || '';
          }
          return val;
        }).filter(Boolean);

        values.push(row.id || '');
        values.push(row.buy[0] ? this.convertTimestampToDate(row.buy[0].when) : '');
        values.push(row.sell[0] ? this.convertTimestampToDate(row.sell[0].when) : '');
        values.push(row.buy[0] ? row.buy[0].tx : '');
        values.push(row.sell[0] ? row.sell[0].tx : '');

        values.push(row.mint[0] ? this.convertTimestampToDate(row.mint[0].when) : '');
        values.push(row.mint[0] ? row.mint[0].tx : '');
        values.push(row.mint[0] ? '$' + parseFloat(row.mint[0]?.usd_price || 0).toFixed(2) : '');
        values.push(row.buy[0] ? '$' + parseFloat(row.buy[0]?.usd_price || 0).toFixed(2) : '');
        values.push(row.sell[0] ? '$' + parseFloat(row.sell[0]?.usd_price || 0).toFixed(2) : '');


        if (row.sell.length > 0) {
          const pnl = this.getPNL(row);
          values.push(pnl.percentage !== null && pnl.percentage !== Infinity ? pnl.percentage.toFixed(2) + '%' : '');
        } else {
          values.push('');
        }

        if (row.sell.length > 0) {
          const pnl = this.getPNL(row);
          values.push(pnl.value !== null && pnl.value !== Infinity ? pnl.value.toFixed(5) : '');
        } else {
          values.push('');
        }

        const pnlUsd = this.getPNLUsd(row);
        values.push(pnlUsd.value !== null && !isNaN(pnlUsd.value) ? '$' + pnlUsd.value.toFixed(2) : '');

        csvRows.push(values.join(';'));
      });

      const csvString = csvRows.join('\r\n');
      const blob = new Blob([csvString], { type: 'text/csv' });
      const url = window.URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.setAttribute('hidden', '');
      a.setAttribute('href', url);
      a.setAttribute('download', 'ordinals_trade_report.csv');

      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
    },
    openAboutCollection() {
      this.dialogAboutCollection = true
    },
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
        this.overallBuyUsd = response.data['ordinals-track'].usd_buy
        this.overallMintUsd = response.data['ordinals-track'].usd_mint
        this.overallSellUsd = response.data['ordinals-track'].usd_sell
        this.overallPnlUsd = response.data['ordinals-track'].usd_pnl
        this.overallSell = response.data['ordinals-track'].sell
        this.overallMint = response.data['ordinals-track'].mint
        this.overallCount = response.data['ordinals-track'].count
        this.overallPnl = response.data['ordinals-track'].pnl
        this.dataTrades = response.data['ordinals-track'].trades
        this.dataCollections = response.data['ordinals-track'].collections
        this.dataHoldings = response.data['ordinals-track'].holdings
        this.loadingData = false
      } catch (e) {
        console.error(e)
      }
      try {
        const response = await axios.get(`https://api.ordinalstracker.io/holdings/${wallets}`, {
          headers: {
            'Content-Type': 'application/json'
          }
        })
        this.dataHoldings = response.data.holdings
        this.loadingDataHoldings = false
        this.totalBalanceOrdinals = response.data.total_balance
        this.totalBalanceOrdinalsUsd = response.data.usd_total_balance
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

.arrows-event {
  background-color: #252424;
}

.arrows-event:hover {
  background-color: #3d3939af;
}

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

.grid-holding {
  border: 0.5px;
  transition: 0.5s;
}

.grid-holding:hover {
  border: 0.5px solid orange;
}

.hover-effect>div {
  border-bottom: solid 2px orange;
  border-left: solid 2px orange;
  border-right: solid 2px orange;
  border-radius: 2px;
  transition: border-color 0.3s;
}

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

.custom-table .q-table__top {
  position: -webkit-sticky;
  position: sticky;
  top: 0;
  z-index: 1;
  background-color: inherit;
}



.custom-table .q-tr:last-child {
  border-bottom: none;
}


.custom-table .q-table__viewport {
  max-height: calc(100% - 52px);
}

.transition-out {
  transform: translateY(-100%);
  opacity: 0;
}
</style>
