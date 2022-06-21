<template>
  <div class="AQI" v-bind:class="getClassAQI()">
    <span class="title">{{ measurement.currentPM2 }}</span>
    <span class="unit">AQI</span>
  </div>
  <div class="AQI" v-bind:class="getClassTemp()">
    <span class="title">{{ measurement.temp }}</span>
    <span class="unit">deg</span>
  </div>
</template>

<script>
import * as CONSTANTS from '../Constants.json';


export default {
  name: 'IQAirHome',
  data: () => ({
    measurement: {
    currentPM2: 0,
    temp: 0,
    city: ''}
  }),
  created() {
    this.fetchData()
    this.timer = setInterval(this.fetchData, 1800000);
  },
  methods: {
    getClassAQI() {
      return {
        'BgGreen': this.measurement.currentPM2 <= 50,
        'BgOrange': this.measurement.currentPM2 > 50 && this.measurement.currentPM2 < 100,
        'BgRed': this.measurement.currentPM2 >= 100
      }
    },
    getClassTemp() {
      return {
        'BgGreen': this.measurement.temp <= 24,
        'BgOrange': this.measurement.temp > 24 && this.measurement.temp < 30,
        'BgRed': this.measurement.temp >= 30
      }
    },
    async fetchData() {
      const url = CONSTANTS.backend + '/iqair/getOutdoorQuality'
      fetch(url)
          .then(result => result.json())
      .then(currentAQIData => {
        this.measurement = currentAQIData
      })
      .catch(err => {
        console.log(err);
        this.city = 'n/a';
        this.aqius = 'n/a';
        this.temp = 'n/a';
      })

    },
    cancelAutoUpdate () {
      clearInterval(this.timer);
    }
  },
  beforeUnmount () {
    this.cancelAutoUpdate();
  }
}

</script>
