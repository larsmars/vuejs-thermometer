<template>
  <div id="app">
    <vue-thermometer :level="40" :min="-20" :max="25"/>
  </div>
</template>

<script>
import VueThermometer from '../src/components/VueThermometer.vue'

export default {
  name: 'app',
  components: {
    VueThermometer
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
