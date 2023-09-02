<template>
  <q-layout view="">
    <!-- <q-header class="bg-orange-6">
      <q-toolbar>
        <q-toolbar-title class="text-bold text-dark">
          Ordinals Tracker
        </q-toolbar-title>

        <div></div>
      </q-toolbar>
    </q-header> -->


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
