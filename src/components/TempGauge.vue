<template>
  <div class="temp-gauge" :style="gaugeStyles" :class="customClass">
    {{ value }} {{ scale }}
  </div>
</template>
<script>
const defaultStyles = {
  left: 0,
  right: 0
}
export default {
  props: {
    value: {
      type: [String, Number],
      default: '0',
      required: false
    },
    scale: {
      type: String,
      default: '°C',
      required: false
    },
    styles: {
      type: Object,
      default: () => ({}),
      required: false
    },
    customClass: {
      type: String,
      required: false
    }
  },
  data () {
    return {
      gaugeStyles: {
        ...defaultStyles,
        ...this.styles
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.temp-gauge {
  background-color: red;
}
</style>
