<template>
  <q-layout class="container">
    <component :if="show" :is="minigame" />
  </q-layout>
</template>

<script>
import { ref } from 'vue'
import Blinking from './components/Blinking.vue'
import Password from './components/Password.vue'
export default {
  name: 'LayoutDefault',

  components: {
    Blinking, Password
  },

  setup () {
    return {
      show: ref(false),
      minigame: ref(Password)
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
