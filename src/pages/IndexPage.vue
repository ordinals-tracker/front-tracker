<template>
  <q-page class="flex bg-dark flex-center">
    <div class="flex gt-sm absolute-bottom justify-end items-right q-px-lg full-width q-py-md">
      <q-toggle color="orange" icon="on" unchecked-icon="switch" :class="tutorials ? 'text-orange' : 'text-white'"
        label="Tutorials" v-model="tutorials"></q-toggle>
    </div>

    <div v-if="!searchingFor" class="column q-gutter-y-lg flex items-center justify-center">
      <div v-if="trackType != null" class="flex justify-center q-py-lg items-center">

        <div v-if="trackType == 'single'" class="flex row gt-sm items-center q-gutter-x-sm">

          <q-input bg-color="orange-6" style="min-width: 350px;" color="dark" outlined autofocus label="Wallet Search"
            v-model="walletToSearch">
          </q-input>
          <q-btn :to="`/track/${walletToSearch}`" :disable="walletToSearch == ''" size="lg" color="orange" flat
            label="search" style="border-radius: 9px;">
          </q-btn>
        </div>
        <div v-if="trackType == 'single'" class="flex full-width justify-center items-center column lt-md items-center q-gutter-y-sm">

          <q-input bg-color="orange-6" style="min-width: 90vw;" color="dark" outlined autofocus label="Wallet Search"
            v-model="walletToSearch">
          </q-input>
          <q-btn :to="`/track/${walletToSearch}`" :disable="walletToSearch == ''" size="lg" color="orange"
            label="search" style="border-radius: 9px;">
          </q-btn>
        </div>

        <div v-if="trackType == 'walletConnect'" class="row items-center flex q-gutter-x-md">

          <q-btn @click="openWalletModal" icon="currency_bitcoin" label="Connect your BTC wallet" text-color="dark"
            style="min-width: 350px; border-radius: 9px;" class="q-pa-sm text-h6 text-bold" color="orange-5"> </q-btn>
        </div>

        <div v-if="trackType == 'multi' && this.walletsToSearch.length > 0"
          class="full-width gt-sm flex-center justify-center items-center flex">
          <div style="max-width: 25vw; " class="flex items-center flex-center q-py-sm justify-center">
            <div class="flex full-width items-center justify-center">
              <div class="flex q-pa-lg items-center flex-center text-white text-bold">
                <div class="full-width flex items-center text-h6 text-orange justify-center q-py-sm">
                  Wallets
                </div>
                <div
                  style="max-height: 105px; overflow-y: scroll; border: 0.5px solid rgba(195, 134, 22, 0.425); border-radius: 3px;"
                  class="q-pa-sm">
                  <div style="height: max-content;" class="column flex justify-center items-center">
                    <q-chip square clickable :label="wallet" style="font-size: 1rem; font-weight: 400;" color="orange"
                      text-color="dark" icon="delete" v-for="(wallet, index) in walletsToSearch" :key="index"
                      class="flex justify-center q-pa-sm items-center flex-center col">
                    </q-chip>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div v-if="trackType == 'multi' && this.walletsToSearch.length > 0"
          class="full-width lt-md  flex-center justify-center items-center flex">
          <div style="width: 90vw; " class="flex items-center flex-center q-py-sm justify-center">
            <div class="flex full-width items-center justify-center">
              <div class="flex q-pa-lg items-center flex-center text-white text-bold">
                <div class="full-width flex items-center text-h6 text-orange justify-center q-py-sm">
                  Wallets
                </div>
                <div
                  style="max-height: 125px; overflow-y: scroll; border: 0.5px solid rgba(195, 134, 22, 0.425); border-radius: 3px;"
                  class="q-pa-sm">
                  <div style="height: max-content;" class="column flex justify-center items-center">
                    <q-chip square clickable :label="wallet" style="font-size: 1rem; font-weight: 400;" color="orange"
                      text-color="dark" icon="delete" v-for="(wallet, index) in walletsToSearch" :key="index"
                      class="flex justify-center q-pa-sm items-center flex-center col">
                    </q-chip>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div v-if="trackType == 'multi'" class="flex row gt-sm items-center q-gutter-x-sm">
          <q-input bg-color="orange-6" style="min-width: 350px;" color="dark" outlined autofocus label="Wallet Add"
            v-model="multiWalet">
          </q-input>
          <q-btn :disable="multiWalet.length < 10" @click="addWalletOnGroup(multiWalet)" size="lg" color="orange" flat
            icon="add" style="border-radius: 9px;">
          </q-btn>
          <q-btn v-if="walletsToSearch.length >= 2" :to="`/track/${walletsToSearch.toString()}`" size="lg" color="orange"
            flat label="Search" style="border-radius: 9px;">
          </q-btn>
        </div>

        <div v-if="trackType == 'multi'" class="flex lt-md row justify-center full-width items-center q-gutter-y-sm">
          <q-input bg-color="orange-6" style="min-width: 90vw;" color="dark" outlined autofocus label="Wallet Add"
            v-model="multiWalet">
          </q-input>
          <q-btn :disable="multiWalet.length < 10" @click="addWalletOnGroup(multiWalet)" size="lg" color="orange"
           label="add" icon="add" style="border-radius: 9px;">
          </q-btn>
          <q-btn v-if="walletsToSearch.length >= 2" :to="`/track/${walletsToSearch.toString()}`" size="lg" color="orange"
            flat label="Search" style="border-radius: 9px;">
          </q-btn>
        </div>

      </div>
      <div v-if="trackType == null"
        class="row gt-sm items-center text-h3 justify-center text-orange-5 text-bold flex q-py-sm">
        Select Your type of Track
      </div>
      <div class="row gt-sm q-gutter-x-lg items-center q-mb-lg">
        <q-btn v-if="trackType != 'single'" @click="(trackType = 'single') && (this.tutorials = false)"
          style=" border-radius: 9px;" icon="search" flat color="orange-5" size="xl" class="">
          <q-tooltip v-if="tutorials" class="bg-transparent gt-sm items-center text-orange-5 text-bold text-h6">
            <video width="720" height="440" autoplay loop muted>
              <source type="video/mp4"
                src="https://cdn.discordapp.com/attachments/459557016042471454/1147370803281797160/2023-09-01_23-54-56.mp4">
            </video>
          </q-tooltip>
          <q-tooltip v-else class="bg-dark gt-sm items-center text-orange-5 text-bold text-h6">
            Single Wallet Search
          </q-tooltip>
        </q-btn>
        <q-btn v-if="trackType != 'walletConnect'" @click="(trackType = 'walletConnect') && (this.tutorials = false)"
          style=" border-radius: 9px;" icon="wallet" flat color="orange-5" size="xl" class="">
          <q-tooltip v-if="tutorials" class="bg-transparent items-center text-orange-5 text-bold text-h6">
            <video width="720" height="440" autoplay loop muted>
              <source type="video/mp4"
                src="https://cdn.discordapp.com/attachments/459557016042471454/1147372312493031434/2023-09-01_23-55-16.mp4">
            </video>
          </q-tooltip>
          <q-tooltip v-else class="bg-dark items-center text-orange-5 text-bold text-h6">
            Connect your wallet to see
          </q-tooltip>
        </q-btn>
        <q-btn v-if="trackType != 'multi'" @click="(trackType = 'multi') && (this.tutorials = false)"
          style=" border-radius: 9px;" icon="filter_2" icon-right="add" flat color="orange-5" size="xl" class="">
          <q-tooltip v-if="tutorials" class="bg-transparent items-center text-orange-5 text-bold text-h6">
            <video width="720" height="440" autoplay loop muted>
              <source type="video/mp4"
                src="https://cdn.discordapp.com/attachments/459557016042471454/1147372533742579802/2023-09-01_23-55-44.mp4">
            </video>
          </q-tooltip>
          <q-tooltip v-else class="bg-dark items-center text-orange-5 text-bold text-h6">
            Search a group of wallets
          </q-tooltip>
        </q-btn>
      </div>
      <div class="column lt-md q-gutter-y-xl items-center q-mb-lg">
        <q-btn icon-right="search" label="Single Wallet" v-if="trackType != 'single'"
          @click="(trackType = 'single') && (this.tutorials = false)" style=" border-radius: 9px;" icon="filter_1" flat
          color="orange-5" size="xl" class="">
        </q-btn>
        <q-btn icon-right="search" label="Multi Wallet" v-if="trackType != 'multi'" @click="(trackType = 'multi') && (this.tutorials = false)"
          style=" border-radius: 9px;" icon="filter_3" flat color="orange-5" size="xl" class="">
        </q-btn>
        <q-btn icon-right="search" label="your wallet" v-if="trackType != 'walletConnect'"
          @click="(trackType = 'walletConnect') && (this.tutorials = false)" style=" border-radius: 9px;" icon="wallet"
          flat color="orange-5" size="xl" class="">

        </q-btn>
      </div>

    </div>

    <q-dialog v-model="walletDialogModal">
      <q-card class="bg-orange gt-sm" style="min-width: 23vw; min-height: 33vh; border-radius: 10px; opacity: .9;">
        <q-card-section style="min-height: 33vh;" class="full-height justify-center flex flex-center items-center">
          <div class="column full-height q-gutter-y-sm justify-center full-width items-center">
            <div v-if="unisatInstalled" class="full-width flex ">
              <q-btn @click="getUnisatWalletAddress" class="full-width q-py-lg justify-center items-center " color="dark"
                style="border: solid 0.5px orange; border-radius: 13px;">
                <div class="row q-gutter-x-sm items-center flex">
                  <div class="">
                    <q-img
                      src="https://cdn.discordapp.com/attachments/459557016042471454/1147338037592870912/125119198-removebg-preview.png"
                      style="width: 30px;"></q-img>
                  </div>
                  <div class="">
                    <div style="white-space: nowrap;" class="text-white flex row text-bold text-h6">
                      Connect Unisat
                    </div>
                  </div>
                </div>
              </q-btn>
            </div>
            <div class="full-width flex ">
              <q-btn @click="getXverseWalletaAddress" class="full-width q-py-lg justify-center items-center " color="dark"
                style="border: solid 0.5px orange; border-radius: 13px;">
                <div class="row q-gutter-x-sm items-center flex">
                  <div class="">
                    <q-img
                      src="https://cdn.discordapp.com/attachments/459557016042471454/1147338037341208636/unnamed-removebg-preview.png"
                      style="width: 30px;"></q-img>
                  </div>
                  <div class="">
                    <div style="white-space: nowrap;" class="text-white flex row text-bold text-h6">
                      Connect Xverse
                    </div>
                  </div>
                </div>
              </q-btn>
            </div>
          </div>
        </q-card-section>
      </q-card>
      <q-card class="bg-orange flex justify-center items-center lt-md" style="min-width: 100%; min-height: 50%; border-radius: 10px; opacity: .9;">
        <q-card-section style="min-height: 33vh;" class="full-height full-width justify-center flex flex-center items-center">
          <div class="column full-height q-gutter-y-sm justify-center full-width items-center">
            <div class="full-width flex ">
              <q-btn @click="getXverseWalletaAddress" class="full-width q-py-lg justify-center items-center " color="dark"
                style="border: solid 0.5px orange; border-radius: 13px;">
                <div class="row q-gutter-x-sm items-center flex">
                  <div class="">
                    <q-img
                      src="https://cdn.discordapp.com/attachments/459557016042471454/1147338037341208636/unnamed-removebg-preview.png"
                      style="width: 30px;"></q-img>
                  </div>
                  <div class="">
                    <div style="white-space: nowrap;" class="text-white flex row text-bold text-h6">
                      Connect Xverse
                    </div>
                  </div>
                </div>
              </q-btn>
            </div>
          </div>
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import { getAddress } from 'sats-connect'

export default defineComponent({
  name: 'IndexPage',
  data() {
    return {
      trackType: null,
      singleSearch: '',
      walletDialogModal: false,
      unisatInstalled: false,
      tutorials: false,
      walletToSearch: '',
      walletsToSearch: [],
      multiWalet: '',
      xverseWalletAddressOrdinals: '',
      xverseWalletAddressPayment: '',
      searchingFor: false
    }
  },
  methods: {
    addWalletOnGroup(address) {
      this.walletsToSearch.push(address)
      this.multiWalet = ''
    },
    openWalletModal() {
      this.walletDialogModal = true
    },
    checkUnisat() {
      if (typeof window.unisat !== 'undefined') {
        this.unisatInstalled = true
        // console.log('UniSat Wallet is installed!');
      }
    },
    async getXverseWalletaAddress() {
      try {
        const getAddressOptions = {
          payload: {
            purposes: ['ordinals', 'payment'],
            message: 'Address for receiving Ordinals and payments',
            network: {
              type: 'Mainnet'
            },
          },
          onFinish: (response) => {
            const ordinalsAddressObj = response.addresses.find(addr => addr.purpose === "ordinals");
            const paymentAddressObj = response.addresses.find(addr => addr.purpose === "payment");

            if (ordinalsAddressObj) {
              this.xverseWalletAddressOrdinals = ordinalsAddressObj.address;
              this.walletToSearch = ordinalsAddressObj.address;
              this.searchingFor = true
              this.$router.push('/track/' + this.walletToSearch)
            } else {
              console.error("Address with purpose 'ordinals' not found!");
            }
            if (paymentAddressObj) {
              this.xverseWalletAddressPayment = paymentAddressObj.address;
            } else {
              console.error("Address with purpose 'ordinals' not found!");
            }

            this.walletDialogModal = false
            // console.log(response);

          },
          onCancel: () => alert('Request canceled'),
        }

        await getAddress(getAddressOptions);

      } catch (e) {
        console.error(e);
      }
    },
    async getUnisatWalletAddress() {
      try {
        let accounts = await window.unisat.requestAccounts();
        console.log('connect success', accounts);
        this.walletToSearch = accounts;
        this.$router.push('/track/' + this.walletToSearch)
      } catch (e) {
        console.log('connect failed');
      }
    }

  },
  mounted() {
    this.checkUnisat()
  }
})
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=VT323&display=swap');

* {
  font-family: 'VT323', monospace;
  margin: 0;
  padding: 0;
}</style>
