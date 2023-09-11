<template>
  <q-layout class="" view="llF hhF ">
    <q-header class="justify-center flex items-center" :class="this.$route.path != '/' ? 'bg-btc': 'bg-dark'">
      <q-toolbar style="max-width: 80vw;" class="flex gt-sm">
        <q-toolbar-title class="text-bold  text-h5 text-orange">
         <div class="q-px-md gt-sm">
           Ordinals Tax Report
         </div>
        </q-toolbar-title>

        <div v-if="this.$route.path != '/'" class="">
          <q-btn to="/" style="border-radius: 9px;" icon="search" icon-right="add" color="orange" flat label="New Track"></q-btn>
        </div>
      </q-toolbar>
      <q-toolbar style="" class="flex lt-md">
        <q-toolbar-title class="text-bold  text-h5 text-orange">
         <div class="q-px-md gt-sm">
           Ordinals Tax Rep
         </div>
        </q-toolbar-title>

        <div v-if="this.$route.path != '/'" class="">
          <q-btn to="/" style="border-radius: 9px;" icon-right="search" color="orange" flat label="New Track"></q-btn>
        </div>
      </q-toolbar>
    </q-header>


    <q-page-container >
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent } from 'vue'


export default defineComponent({
  name: 'MainLayout',

  components: {
  },

  setup () {

    return {

    }
  }
})
</script>

<style>
.bg-btc {
  background-color: #252424;
}

::-webkit-scrollbar {
  display: none;
}

</style>
