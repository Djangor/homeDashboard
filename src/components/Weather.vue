<template>
  <div class="Weather">
    <h1>Weather data</h1>
    <div class="todayRow">
      <div class="weatherNow"><i class="wi" v-bind:class="weatherData.currentWeatherType"></i></div>
      <div class="weatherUpcoming"><i class="wi" v-bind:class="weatherData.upcomingWeatherType"></i></div>
    </div>
    <div class="weatherForecastRow">
      <div v-for="entry in weatherData.forecast" :key="entry" class="weatherForecast">
        <i class="wi" v-bind:class="entry.weatherType"></i>
      </div>
    </div>
  </div>
</template>

<script>
import * as CONSTANTS from '../Constants.json';

export default {
  name: 'GetWeather',
  data: () => ({
    weatherData: {}
  }),
  created() {
    this.fetchData()

    this.timer = setInterval(this.fetchData, 3600000);
  },
  methods: {
    async fetchData() {
      const url = CONSTANTS.backend + '/weather/getWeather'
      fetch(url)
          .then(result => result.text()
          )
          .then(data => {
            this.weatherData = JSON.parse(data);
            console.log(data);
          })
          .catch(err => {
            console.log(err);
            this.bringArray = {}

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

<style>
.weatherForecastRow {
  position: relative;
}
.weatherNow {
  font-size: 72pt;
  margin-top:-20px;
}
.weatherUpcoming {
  font-size: 48pt;
}
.weatherForecast {
  font-size:24pt;
}
.weatherNow, .weatherUpcoming, .weatherForecast {
  padding-right:15px;
  float: left;
}
</style>
