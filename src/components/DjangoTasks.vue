<template>
  <div class="DjangoTasks">
    <table>
      <thead v-if="taskArray.length">
      <tr>
        <th style="text-decoration: underline">Todo:</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="entry in taskArray" :key="entry">
        <td>
          {{entry.title}}
        </td>
        <td style="padding-left: 15px">
          {{entry.dueDateF}}
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

function formatEntries(entryArray) {
  entryArray.map(entry => {
    if (entry.dueDate) {
      let dueDate = new Date(entry.dueDate);
      let dueDateF = pad(dueDate.getDate()) + '.' + pad((dueDate.getMonth() + 1)) + ' ' + pad(dueDate.getHours()) + ':' + pad(dueDate.getMinutes());
      entry.dueDateF = dueDateF;
    }
  });
  return entryArray;
}

export default {
  name: 'DjangoTasks',
  data: () => ({
    taskArray: Array
  }),
  created() {
    this.fetchData()

    this.timer = setInterval(this.fetchData, 3600000);
  },
  methods: {
    async fetchData() {
      const url = CONSTANTS.backend + '/google/getTaskEntries'
      fetch(url)
          .then(result => result.text()
          )
          .then(entries => {
            this.taskArray = formatEntries(JSON.parse(entries));
          })
          .catch(err => {
            console.log(err);
            this.taskArray = []

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
