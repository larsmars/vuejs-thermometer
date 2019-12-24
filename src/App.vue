<template>
  <div id="app">
    <vue-thermometer :value="15" :min="-20" :max="25"/>
    <p>Value: 15, Min: -20, Max: 25 </p>
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
