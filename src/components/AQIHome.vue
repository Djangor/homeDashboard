<template>
  <div class="AQI" v-bind:class="getClassPM2()">
    <span class="title">{{ currentPM2 }}</span>
    <span class="unit">PM2</span>
  </div>
  <div class="AQI" v-bind:class="getClassCO2()">
    <span class="title">{{ currentCO2 }}</span>
    <span class="unit">CO2</span>
  </div>
</template>

<script>
import * as CONSTANTS from '../Constants.json';

export default {
  name: 'IQAirHome',
  data: () => ({
    currentPM2: 0,
    currentCO2: 0
  }),
  created() {
    this.fetchData()
    this.timer = setInterval(this.fetchData, 1800000);
  },
  methods: {
    getClassPM2() {
      return {
        'BgGreen': this.currentPM2 <= 12,
        'BgOrange': this.currentPM2 > 12 && this.currentPM2 < 35,
        'BgRed': this.currentPM2 >= 35
      }
    },
    getClassCO2() {
      return {
        'BgGreen': this.currentCO2 <= 800,
        'BgOrange': this.currentCO2 > 800 && this.currentCO2 < 1200,
        'BgRed': this.currentCO2 >= 1200
      }
    },
    async fetchData() {
      const url = CONSTANTS.IQAirDevice
      fetch(url)
          .then(data => data.json())
          .then(currentAQIData => {
            console.log(JSON.stringify(currentAQIData));
            this.currentPM2 = currentAQIData.current.p2
            this.currentCO2 = currentAQIData.current.co
          })
      .catch(err => {
        console.log(err);
        this.currentCO2 = 'n/a';
        this.currentPM2 = 'n/a';
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
