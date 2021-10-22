<template>
  <q-layout class="container">
    <component :if="show" :is="minigame" />
  </q-layout>
</template>

<script>
import { ref } from 'vue'
import Blinking from './components/Blinking.vue'
import Password from './components/Password.vue'
import Triangle from './components/Triangle.vue'
export default {
  name: 'LayoutDefault',

  components: {
    Blinking, Password, Triangle
  },

  setup () {
    return {
      show: ref(false),
      minigame: ref(Triangle)
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
