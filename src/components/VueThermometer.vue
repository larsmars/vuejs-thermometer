<template>
  <div class="vue-thermometer" :class="customClass">
    {{ value }} {{ scale }}
    <svg xmlns="http://www.w3.org/2000/svg" width="256" height="512">
    <g>
      <g id="g2984"> <!-- ticks/lines -->
        <path v-for="(tick, index) in ticks" :key="index" :stroke="defaultOptions.thermo.tickColor" :stroke-width="defaultOptions.thermo.tickWidth" :stroke-miterlimit="defaultOptions.thermo.tickWidth" :id="'path2931' + index" :d="offsetLine(index)"/>
      </g>
      <!-- set the height attribute to something between 0 and 10.  Can be floating-point. -->
      <!-- Frame round thermo black/white-->
      <path fill="#fcf9f9" fill-rule="nonzero" stroke="black" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="4" stroke-dashoffset="0" id="rect2968" d="m41.414581,33c-9.255863,0 -16.707302,7.136002 -16.707302,16l0,372.28125c-9.987129,5.535095 -16.707279,15.880676 -16.707279,27.71875c0,17.664001 14.96973,32 33.414581,32c18.444828,0 33.414551,-14.335999 33.414551,-32c0,-11.838074 -6.720184,-22.183655 -16.707272,-27.71875l0,-372.28125c0,-8.863998 -7.45145,-16 -16.707279,-16z"/>
      <rect fill="#fcf9f9" fill-rule="nonzero" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="4" stroke-dashoffset="0" ry="16" rx="16" y="33" x="24.707283" height="416" width="33.414566" id="rect2922"/>
      <rect fill="#fcf9f9" fill-rule="nonzero" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="4" stroke-dashoffset="0" y="385" x="24.707283" height="64" width="33.414558" id="rect3021"/>
      <path fill="#fcf9f9" fill-rule="nonzero" stroke-width="4" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="4" stroke-dashoffset="0" d="m74.829132,449a33.41457,32 0 1 1 -66.829132,0a33.41457,32 0 1 1 66.829132,0z" id="path2924"/>

      <!-- this is the round buttom thing -->
      <text transform="rotate(-0.000146047, 188.988, 214.35) matrix(6.31169, 0, 0, 0.394647, -808.247, 123.338)" xml:space="preserve" text-anchor="middle" font-family="Arial" font-size="24" id="svg_1" y="239" x="158" stroke-width="0" stroke="#000000" fill="#000000"/>
      <text transform="matrix(8.21429, 0, 0, 1, -1673.71, 0)" xml:space="preserve" text-anchor="middle" font-family="Arial" font-size="0.001" id="svg_3" y="186" x="225" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">5000</text>
      <ellipse fill="#FF0000" stroke="#000000" stroke-width="0" cx="41.499996" cy="448.500003" id="svg_2" rx="29.000001" ry="28.499999"/>

      <!-- <text :fill="defaultOptions.text.color" :stroke="defaultOptions.text.color" stroke-width="0" x="143.5" y="74" id="svg_4" font-size="24" font-family="Arial" text-anchor="middle" xml:space="preserve">100%</text> -->

      <!-- this is the bar/temp height -->
      <ellipse fill="#FF0000" stroke="#000000" stroke-width="0" cx="40.499996" cy="448.500003" rx="29.000001" ry="28.499999" id="svg_5"/>
      <ellipse fill="#FF0000" stroke="#000000" stroke-width="0" cx="40.499996" cy="449.500003" rx="29.000001" ry="28.499999" id="svg_6"/>
      <ellipse fill="#FF0000" stroke="#000000" stroke-width="0" cx="41.999996" cy="449.500003" rx="29.5" ry="28.499999" id="svg_7"/>
      <rect fill="#FF0000" stroke="#000000" stroke-width="0" x="27.5" y="127.00036" width="27" height="296.999941" id="svg_8"/>
      <rect fill="#FF0000" stroke="#000000" stroke-width="0" x="71.5" y="358" width="46" height="0" id="svg_9"/>
      <rect fill="#FF0000" stroke="#000000" stroke-width="0" x="54.5" y="320" width="64" height="0" id="svg_10"/>

      <!-- this is the temp values -->
      <text v-for="(tick, index) in ticks" :key="index" :id="'svg_10' + index" :fill="defaultOptions.text.color" :stroke="defaultOptions.text.color" stroke-width="0" x="138.5" :y="offset(74, index)" :font-size="defaultOptions.text.fontSize" :font-family="defaultOptions.text.fontFamily" text-anchor="middle" xml:space="preserve">{{ tick }}</text>
    </g>
    </svg>
  </div>
</template>

<script>
const _defaultStep = 32

export default {
  props: {
    value: {
      type: Number,
      default: 0,
      required: true
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
        shadowEnable: true,
        shadowColor: '#000000',
        fontSize: 24,
        fontFamily: 'Helvetica',
        dynamicPosition: false,
        hideText: false
      },
      thermo: {
        color: '#2dbd2d',
        backgroundColor: '#C0C0C0',
        ticks: 10,
        tickColor: 'black',
        tickWidth: '4'
      },
      layout: {
        height: 35,
        width: 140,
        verticalTextAlign: 61,
        horizontalTextAlign: 43,
        zeroOffset: 0,
        strokeWidth: 30,
        progressPadding: 0,
        type: 'line'
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
    tickStep () {
      return (Math.abs(this.min) + Math.abs(this.max)) / this.defaultOptions.thermo.ticks
    },
    ticks () {
      let ticks = []
      let maxValue = this.max
      for (let i = 0; i < this.defaultOptions.thermo.ticks; i++) {
        ticks.push(Math.ceil(maxValue))
        maxValue -= this.tickStep
      }
      return ticks
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
    offset (y, index) {
      let offset = index * _defaultStep
      return (y + offset)
    },
    offsetLine (index) {
      let base = 65
      let offset = index * _defaultStep
      let length = index % 2 === 0 ? 'l20' : 'l40'
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
  background-color: gray;
}
</style>
