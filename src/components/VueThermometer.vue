<template>
  <div class="vue-thermometer" :class="customClass">
    <svg xmlns="http://www.w3.org/2000/svg" :width="width" :height="height">
    <g>
      <g id="g2984"> <!-- ticks/lines -->
        <path v-for="(tick, index) in ticks" :key="index" :stroke="defaultOptions.thermo.tickColor" :stroke-width="defaultOptions.thermo.tickWidth" :stroke-miterlimit="defaultOptions.thermo.tickWidth" :id="'path2931' + index" :d="offsetLine(index)"/>
      </g>
      <!-- set the height attribute to something between 0 and 10.  Can be floating-point. -->
      <!-- Frame round thermo black/white-->
      <!-- <path fill="#fcf9f9" fill-rule="nonzero" stroke="black" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="4" stroke-dashoffset="0" id="rect2968" d="m41.414581,33c-9.255863,0 -16.707302,7.136002 -16.707302,16l0,372.28125c-9.987129,5.535095 -16.707279,15.880676 -16.707279,27.71875c0,17.664001 14.96973,32 33.414581,32c18.444828,0 33.414551,-14.335999 33.414551,-32c0,-11.838074 -6.720184,-22.183655 -16.707272,-27.71875l0,-372.28125c0,-8.863998 -7.45145,-16 -16.707279,-16z"/> -->
      <rect :fill="defaultOptions.thermo.backgroundColor" fill-rule="nonzero" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="4" stroke-dashoffset="0" ry="16" rx="16" :y="glassOffset" x="24.707283" :height="glassHeight" :width="glassWidth" id="rect2922"/>
      <path :fill="defaultOptions.thermo.backgroundColor" fill-rule="nonzero" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="4" stroke-dashoffset="0" d="m74.829132,449a33.41457,32 0 1 1 -66.829132,0a33.41457,32 0 1 1 66.829132,0z" id="path2924"/>

      <!-- this is the round buttom thing -->
      <ellipse :fill="defaultOptions.thermo.color" stroke="#000000" stroke-width="0" cx="41.499996" cy="448.500003" id="svg_2" rx="29.000001" ry="28.499999"/>

      <!-- this is round buttom wrapper -->
      <ellipse :fill="defaultOptions.thermo.color" stroke="#000000" stroke-width="0" cx="40.499996" cy="448.500003" rx="29.000001" ry="28.499999" id="svg_5"/>
      <ellipse :fill="defaultOptions.thermo.color" stroke="#000000" stroke-width="0" cx="40.499996" cy="449.500003" rx="29.000001" ry="28.499999" id="svg_6"/>
      <ellipse :fill="defaultOptions.thermo.color" stroke="#000000" stroke-width="0" cx="41.999996" cy="449.500003" rx="29.5" ry="28.499999" id="svg_7"/>

      <!-- this is the bar/temp height -->
      <rect :fill="defaultOptions.thermo.color" stroke="#000000" stroke-width="0" x="27.5" :y="thermoOffset" :width="thermoWidth" :height="thermoHeight" id="svg_8"/>
      <!-- this is the temp values -->
      <text v-for="(tick, index) in ticks" :key="index" :id="'svg_10' + index" :fill="defaultOptions.text.color" :stroke="defaultOptions.text.color" stroke-width="0" x="128.5" :y="offsetText(index)" :font-size="defaultOptions.text.fontSize" :font-family="defaultOptions.text.fontFamily" text-anchor="middle" xml:space="preserve">{{ tick }}{{ scale}}</text>
    </g>
    </svg>
  </div>
</template>

<script>

export default {
  props: {
    level: {
      type: Number,
      default: 100,
      required: false
    },
    min: {
      type: Number,
      default: -10,
      required: false
    },
    max: {
      type: Number,
      default: 20,
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
        fontSize: 24,
        textAdjustment: 6,
        fontFamily: 'Arial'
      },
      thermo: {
        color: '#FF0000',
        backgroundColor: '#fcf9f9',
        ticks: 10,
        tickColor: 'black',
        tickWidth: '2'
      },
      layout: {
        height: 700,
        width: 160
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
    width () {
      return this.defaultOptions.layout.width
    },
    height () {
      return this.defaultOptions.layout.height
    },
    tickStep () {
      return (Math.abs(this.min) + Math.abs(this.max)) / (this.defaultOptions.thermo.ticks - 1)
    },
    ticks () {
      let ticks = []
      let maxValue = this.max
      for (let i = 0; i < this.defaultOptions.thermo.ticks; i++) {
        ticks.push(Math.ceil(maxValue))
        maxValue -= this.tickStep
      }
      return ticks
    },
    thermoWidth () {
      return this.defaultOptions.layout.width / 6
    },
    glassWidth () {
      return (this.defaultOptions.layout.width / 6) + 6
    },
    tickWidth () {
      return Math.ceil((this.defaultOptions.layout.width) / 10)
    },
    glassOffset () {
      return this.defaultOptions.layout.height * 0.025
    },
    glassHeight () {
      return this.defaultOptions.layout.height * 0.95
    },
    tickStepSize () {
      return (this.glassHeight / this.defaultOptions.thermo.ticks)
    },
    thermoOffset () {
      const _topOffset = 5
      let offset = this.glassHeight - this.thermoHeight
      return this.glassOffset + _topOffset + offset
    },
    thermoHeight () {
      return (this.level * (this.glassHeight / 100))
    },
    svgHeight () {
      return this.defaultOptions.layout.height + 100
    },
    svgWidth () {
      return this.defaultOptions.layout.width + 70
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
      let base = (this.tickStepSize / this.defaultOptions.thermo.ticks) + this.glassOffset + this.defaultOptions.text.textAdjustment
      let offset = index * this.tickStepSize
      return (Number(offset) + Number(base))
    },
    offsetLine (index) {
      let base = (this.tickStepSize / this.defaultOptions.thermo.ticks) + this.glassOffset
      let offset = index * this.tickStepSize
      let length = index % 2 === 0 ? 'l' + Math.ceil(this.tickWidth) : 'l' + Math.ceil(this.tickWidth + this.tickWidth)
      offset = Number(offset) + Number(base) + length
      return 'm58.121861,' + offset + '.121853,0'
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
  display: inline-flex;
  background-color: gray;
}
</style>
