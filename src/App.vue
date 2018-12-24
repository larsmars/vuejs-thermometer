<template>
  <div id="app">
    <vue-temp-gauge/>
  </div>
</template>

<script>
import VueTempGauge from '../src/components/VueTempGauge.vue'

export default {
  name: 'app',
  components: {
    VueTempGauge
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
