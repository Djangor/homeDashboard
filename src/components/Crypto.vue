<template>
  <div class="CryptoView">
    <table>
      <thead v-if="symbols.length">
      <tr>
        <th style="text-decoration: underline" colspan="2">exchangeRates:</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="entry in symbols" :key="entry">
        <td>
          {{entry.symbol}}
        </td>
        <td style="padding-left: 15px">
          {{entry.rate}}
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import * as CONSTANTS from '../Constants.json';

export default {
  name: 'CryptoView',
  data: () => ({
    symbols: Array
  }),
  created() {
    this.fetchData()

    this.timer = setInterval(this.fetchData, 3600000);
  },
  methods: {
    async fetchData() {
      const url = CONSTANTS.backend + '/crypto/getRates'
      fetch(url)
          .then(result => result.text()
          )
          .then(entries => {
            this.symbols = JSON.parse(entries);
          })
          .catch(err => {
            console.log(err);
            this.symbols = []

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
