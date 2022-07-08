<template>
  <div class="datetime">
    <div class="time">{{ time }}</div>
    <div class="date">{{ date }}</div>
  </div>
</template>

<script>


function pad(n){
  return n<10 ? '0'+n : n}

function translateDay(day) {
  switch (day) {
    case 0: return 'Sunday';
    case 1: return 'Monday';
    case 2: return 'Tuesday';
    case 3: return 'Wednesday';
    case 4: return 'Thursday';
    case 5: return 'Friday';
    case 6: return 'Saturday';
    default: return 'whadda';
  }
}

export default {
  name: 'DateTime',
  data: () => ({
    date: '---',
    time: '-:-'
  }),
  created() {
    this.fetchData()
    this.timer = setInterval(this.fetchData, 5000);
  },
  methods: {
    async fetchData() {
      const date = new Date();
      this.time = pad(date.getHours()) + ':' + pad(date.getMinutes());
      this.date = translateDay(date.getDay()) + ' ' + pad(date.getDate()) + '.' + pad(date.getMonth() + 1) + '.' + date.getFullYear();
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

.datetime {
  text-align: left;
  top:-20px;
  position:relative;
}

.time {
  margin-bottom: -20px;
  font-size: 72pt;
}

.date {
  font-size: large;
}

</style>
