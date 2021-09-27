<template>
  <q-layout class="container">
    <component :if="show" :is="minigame" />
  </q-layout>
</template>

<script>
import { ref } from 'vue'
import Blinking from './components/Blinking.vue'

export default {
  name: 'LayoutDefault',

  components: {
    Blinking
  },

  setup () {
    return {
      show: ref(false),
      minigame: ref(Blinking)
    }
  }
}
</script>

<style scoped>
  .container {
    width: 100%;
    height: 100%;
    display: flex;
  }
</style>
