<template>
  <div class="vue-thermometer" :class="customClass">
    <svg xmlns="http://www.w3.org/2000/svg" :width="width" :height="height">
    <g>
      <g id="11"> <!-- ticks/lines -->
        <path v-show="defaultOptions.thermo.ticksEnabled" v-for="(tick, index) in ticks" :key="index" :stroke="defaultOptions.thermo.tickColor" :stroke-width="defaultOptions.thermo.tickWidth" :stroke-miterlimit="defaultOptions.thermo.tickWidth" :id="'12' + index" :d="offsetLine(index)"/>
      </g>
      <!-- Frame round thermo black/white-->
      <circle :cx="roundDotPositionX" :cy="glassHeight" :r="(glassWidth * 0.9) + 2" :stroke="defaultOptions.thermo.frameColor" stroke-width="4" :fill="defaultOptions.thermo.color" />
      <rect :fill="defaultOptions.thermo.frameColor" fill-rule="nonzero" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="4" stroke-dashoffset="0" ry="16" rx="16" :y="glassOffset - 2" :x="baseXOffset - 2" :height="glassHeight + 4" :width="glassWidth + 4" id="14"/>
      <rect :fill="defaultOptions.thermo.backgroundColor" fill-rule="nonzero" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="4" stroke-dashoffset="0" ry="16" rx="16" :y="glassOffset" :x="baseXOffset" :height="glassHeight" :width="glassWidth" id="15"/>

      <!-- this is the round buttom thing center of it (red) -->
      <circle :cx="roundDotPositionX" :cy="glassHeight" :r="glassWidth * 0.9" :stroke="defaultOptions.thermo.backgroundColor" stroke-width="4" :fill="defaultOptions.thermo.color" />

      <!-- this is the bar/temp height -->
      <rect :fill="defaultOptions.thermo.color" stroke="#000000" stroke-width="0" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="4" stroke-dashoffset="0" ry="8" rx="8" :x="baseXOffset + 3" :y="thermoOffset" :width="thermoWidth" :height="thermoHeight" id="svg_18"/>
      <!-- this is the temp values -->
      <text v-show="defaultOptions.text.textEnabled" v-for="(tick, index) in ticks" :key="index" :id="'svg_19' + index" :fill="defaultOptions.text.color" :stroke="defaultOptions.text.color" stroke-width="0" :x="textSpacing" :y="offsetText(index)" :font-size="defaultOptions.text.fontSize" :font-family="defaultOptions.text.fontFamily" text-anchor="middle" xml:space="preserve">{{ tick }}{{ scale}}</text>
    </g>
    </svg>
  </div>
</template>

<script>
const _textOffset = 0.75

export default {
  props: {
    value: {
      type: Number,
      default: 0,
      required: false
    },
    min: {
      type: Number,
      default: -20,
      required: false
    },
    max: {
      type: Number,
      default: 25,
      required: false
    },
    scale: {
      type: String,
      default: '°C',
      required: false
    },
    options: {
      type: Object,
      required: false
    },
    customClass: {
      type: String,
      required: false
    }
  },
  created () {
    this.defaultOptions = {
      text: {
        color: 'black',
        fontSize: 10,
        textAdjustmentY: 2,
        fontFamily: 'Arial',
        textEnabled: true
      },
      thermo: {
        color: '#FF0000',
        backgroundColor: '#fcf9f9',
        frameColor: 'black',
        ticks: 10,
        ticksEnabled: true,
        tickColor: 'black',
        tickWidth: '1'
      },
      layout: {
        height: 300,
        width: 90
      }
    }
  },
  mounted () {
    if (this.options !== null && this.options !== undefined) {
      this.mergeDefaultOptionsWithProp(this.options)
    }
  },
  data () {
    return {
      defaultOptions: Object
    }
  },
  computed: {
    baseXOffset () {
      return this.defaultOptions.layout.width / 5
    },
    width () {
      return this.defaultOptions.layout.width
    },
    height () {
      return this.defaultOptions.layout.height
    },
    textSpacing () {
      return this.width * _textOffset
    },
    tickStep () {
      return (Math.abs(this.max - this.min)) / (this.defaultOptions.thermo.ticks - 1)
    },
    ticks () {
      let ticks = []
      let maxValue = this.max
      for (let i = 0; i < (this.defaultOptions.thermo.ticks - 1); i++) {
        ticks.push(Math.round(maxValue))
        maxValue -= this.tickStep
      } ticks.push(this.min)
      return ticks
    },
    thermoWidth () {
      return this.defaultOptions.layout.width / 6
    },
    glassWidth () {
      return (this.defaultOptions.layout.width / 6) + 6
    },
    tickWidth () {
      return Math.ceil((this.defaultOptions.layout.width) / 12)
    },
    glassOffset () {
      return this.defaultOptions.layout.height * 0.02
    },
    glassHeight () {
      let height = this.defaultOptions.layout.height * 0.95
      while ((this.defaultOptions.layout.height - height) < 30) {
        height -= 1
      }
      return height
    },
    tickStepSize () {
      return (this.glassHeight / this.defaultOptions.thermo.ticks)
    },
    thermoOffset () {
      let offset = Math.ceil(this.glassHeight - this.thermoHeight)
      return (this.glassOffset + offset)
    },
    level () {
      return Math.ceil(((this.value - this.min) * 100) / (this.max - this.min))
    },
    thermoHeight () {
      return (this.level * ((this.glassHeight) / 100)) + ((((100 - this.level) / 100) * this.glassHeight) * 0.075)
    },
    roundDotPositionX () {
      return this.baseXOffset + this.glassWidth * 0.5
    },
    roundDot () {
      return 'm74.829132,' + this.glassHeight + 'a33.41457,32 0 1 1 -66.829132,0a33.41457,32 0 1 1 66.829132,0z'
    }
  },
  methods: {
    mergeDefaultOptionsWithProp: function (options) {
      var result = this.defaultOptions
      for (var option in options) {
        if (options[option] !== null && typeof (options[option]) === 'object') {
          for (var subOption in options[option]) {
            if (options[option][subOption] !== undefined && options[option][subOption] !== null) {
              result[option][subOption] = options[option][subOption]
            }
          }
        } else {
          result[option] = options[option]
        }
      }
    },
    offsetText (index) {
      let base = (this.tickStepSize / this.defaultOptions.thermo.ticks) + this.glassOffset + this.defaultOptions.text.textAdjustmentY
      let offset = index * this.tickStepSize
      return (Number(offset) + Number(base))
    },
    offsetLine (index) {
      let base = (this.tickStepSize / this.defaultOptions.thermo.ticks) + this.glassOffset
      let offsetY = index * this.tickStepSize
      let length = index % 2 === 0 ? 'l' + Math.ceil(this.tickWidth * 1.4) : 'l' + Math.ceil(this.tickWidth + this.tickWidth)
      offsetY = Number(offsetY) + Number(base) + length
      let offsetX = 'm' + Number(this.defaultOptions.layout.width * 0.4) + '.121861,'
      return offsetX + offsetY + '.121853,0' // todo this fix x offset
    }
  },
  watch: {
    options: function (val) {
      if (val !== null && val !== undefined) {
        this.mergeDefaultOptionsWithProp(val)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.vue-thermometer {
  display: flex;
}
</style>
