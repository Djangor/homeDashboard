<template>
  <div class="Weather">
      <div class="weatherNow"><i class="wi weatherBubble" v-bind:class="weatherData.currentWeatherType"></i></div>
      <div class="weatherUpcoming"><i class="wi" v-bind:class="weatherData.upcomingWeatherType"></i></div>

      <div v-for="entry in weatherData.forecast" :key="entry" class="weatherForecast">
        <i class="wi" v-bind:class="entry.weatherType"></i>
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

.weatherNow {
  font-size: 72pt;
  width: 150px;
  height: 150px;
  margin-top: 5px;
  float: left;
  margin-left: 15px;

}
.weatherUpcoming {
  font-size: 48pt;
  padding-right:15px;
  float: left;

}
.weatherForecast {
  font-size:24pt;
  padding-right:15px;
  float: left;

}
.weatherBubble {
  top: -17px;
  left: 17px;
  position: relative;
}
</style>
