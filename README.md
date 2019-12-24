# vuejs-thermometer
Thermometer component for vue.js <br>
v0.1.2

<img src="https://img.shields.io/badge/license-MIT-green.svg" /> <img src="https://img.shields.io/badge/dependencies-0-brightgreen.svg" /> <img src="https://img.shields.io/badge/bugs-0-red.svg" />

[Live demo](http://softwarefun.no/#/thermometer)
<br>

<img src="http://softwarefun.no/demo_temp.png" height="300">

Do you have questions or want a new feature? Use the "Issues" section :point_left:

## Setup
Install:
```bash
  npm install vuejs-thermometer --save
```

Import: (in your main.js)
```javascript
import VueThermometer from 'vuejs-thermometer'
Vue.use(VueThermometer)
```
## Usage
Use: (in your local .vue file/component, html section)

```xml
    <vue-thermometer
      :level="40"
      :min="-20"
      :max="25"
    />

<!-- Options struct: -->
options: {
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
```

### Properties

| Name            | Type             | Default      | Description            |
| ---             | ---              | ---          | ---                    |
| value           | Number           | 0            | Value of temp |
| min             | Number           | -20          | Min value|
| max             | Number           | 25           | Max value |
| scale           | String           | °C           | Scale format |
| options         | Object           | Object       | Object struct |
| customClass     | String           | N/A          | Custom css class |
