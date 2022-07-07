<template>
  <div class="Bring">
    <ul>
        <li class="" v-for="entry in bringArray" :key="entry">
            {{entry.name}}
        </li>
    </ul>
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

<style>
.bringCard {
  align-items: center;
  display: flex;
  min-width: 100px;
  max-width: 100px !important;
  min-height: 100px;
  margin-right: 5px;
  margin-bottom: 5px;
  text-align: center;
}
.bringText {
  margin: auto;
  height:100%;
  width:100%
}
</style>
