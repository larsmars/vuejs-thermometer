<template>
  <div id="app">
    <vue-thermometer :value="17" :min="-20" :max="30"/>
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
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
