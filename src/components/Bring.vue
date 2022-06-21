<template>
  <div class="Bring">
    <h1>Bring Items</h1>
    <table>
      <tbody>
      <tr v-for="entry in bringArray" :key="entry">
        <td>
          {{entry.name}}
          <span v-if="entry.specification > ''">
            &nbsp;({{entry.specification}})</span>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import * as CONSTANTS from '../Constants.json';

export default {
  name: 'BringItems',
  data: () => ({
    bringArray: Array
  }),
  created() {
    this.fetchData()

    this.timer = setInterval(this.fetchData, 3600000);
  },
  methods: {
    async fetchData() {
      const url = CONSTANTS.backend + '/bring/getItems'
      fetch(url)
          .then(result => result.text()
          )
          .then(entries => {
            this.bringArray = JSON.parse(entries);
            console.log(entries);
          })
          .catch(err => {
            console.log(err);
            this.bringArray = []

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
