<template>
  <div class="DjangoCalendar">
    <table>
      <thead v-if="calArray.today.length">
      <tr>
        <th style="text-decoration: underline">Today:</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="entry in calArray.today" :key="entry">
        <td>
          {{entry.start}}
        </td>
        <td style="padding-left: 15px">
          {{entry.title}}
        </td>
      </tr>
      </tbody>
      <thead v-if="calArray.thisWeek.length">
      <tr>
        <th style="text-decoration: underline">This week:</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="entry in calArray.thisWeek" :key="entry">
        <td>
          {{entry.start}}
        </td>
        <td style="padding-left: 15px">
          {{entry.title}}
        </td>
      </tr>
      </tbody>
      <thead  v-if="calArray.thisMonth.length">
      <tr>
        <th style="text-decoration: underline">This Month:</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="entry in calArray.thisMonth" :key="entry">
        <td>
          {{entry.start}}
        </td>
        <td style="padding-left: 15px">
          {{entry.title}}
        </td>
      </tr>
      </tbody>
      <thead v-if="calArray.rest.length">
      <tr>
        <th style="text-decoration: underline">Later:</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="entry in calArray.rest" :key="entry">
        <td>
          {{entry.start}}
        </td>
        <td style="padding-left: 15px">
          {{entry.title}}
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import * as CONSTANTS from '../Constants.json';

Date.prototype.getWeek = function() {
  var date = new Date(this.getTime());
  date.setHours(0, 0, 0, 0);
  // Thursday in current week decides the year.
  date.setDate(date.getDate() + 3 - (date.getDay() + 6) % 7);
  // January 4 is always in week 1.
  var week1 = new Date(date.getFullYear(), 0, 4);
  // Adjust to Thursday in week 1 and count number of weeks from date to week1.
  return 1 + Math.round(((date.getTime() - week1.getTime()) / 86400000
      - 3 + (week1.getDay() + 6) % 7) / 7);
}


function pad(n){
  return n<10 ? '0'+n : n}

function isToday(date) {
  const today = new Date()
  return date.getDate() == today.getDate() &&
      date.getMonth() == today.getMonth() &&
      date.getFullYear() == today.getFullYear()
}

function isThisWeek(date) {
  const today = new Date();
  return date.getWeek() === today.getWeek() && date.getFullYear() === today.getFullYear()
}

function isThisMonth(date) {
  const today = new Date()
  return date.getMonth() == today.getMonth() &&
      date.getFullYear() == today.getFullYear()
}

function formatEntries(entryArray) {
  const today = [];
  const thisWeek = [];
  const thisMonth = [];
  const rest = [];
  entryArray.forEach(entry => {
    let startDate = new Date(entry.start);
    let start = pad(startDate.getDate()) + '.' + pad((startDate.getMonth() + 1)) + ' ' + pad(startDate.getHours()) + ':' + pad(startDate.getMinutes());
    const formattedEntry = {start: start, title: entry.title.length > 80 ? entry.title.substr(0,77) + '...' : entry.title};
    if (isToday(startDate)) {
      today.push(formattedEntry);
    } else if (isThisWeek(startDate)) {
      thisWeek.push(formattedEntry)
    } else if (isThisMonth(startDate)) {
      thisMonth.push(formattedEntry)
    } else {
      rest.push(formattedEntry);
    }
  });
  return {today: today, thisWeek: thisWeek, thisMonth: thisMonth, rest: rest};
}

export default {
  name: 'DjangoCalendar',
  data: () => ({
    calArray: {today: Array, thisWeek: Array, thisMonth: Array, rest: Array}
  }),
  created() {
    this.fetchData()

    this.timer = setInterval(this.fetchData, 3600000);
  },
  methods: {
    async fetchData() {
      const url = CONSTANTS.backend + '/google/getCalendarEntries'
      fetch(url)
          .then(result => result.text()
          )
          .then(entries => {
            this.calArray = formatEntries(JSON.parse(entries));
          })
          .catch(err => {
            console.log(err);
            this.calArray = {today: [], thisWeek: [], thisMonth: [], rest: []}

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
