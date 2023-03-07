<template>
  <div class="container">
    <div class="row">
    </div>
   <div class="table-responsive">
    <table class="table table-striped table-inverse table-sm dark-bg">
      <tbody>
      <market-item v-for="m in markets"
                   v-bind:market="m"
                   v-bind:show-open-markets="showOpenMarkets"
                   v-bind:show-closed-markets="showClosedMarkets"
                   :key="m.id">
      </market-item>
      </tbody>
    </table>
   </div>

  </div>

</template>

<script>

  import MarketItem from './MarketItem.vue'
  import marketData from '../../static/markets.json'

  // By using a hidden field we encourage vuejs to refresh the DOM style classes
  function refreshMarkets (markets) {
    markets.map(m => {
      m.hidden = '' + new Date().getTime()
      return m
    })
  }

  refreshMarkets(marketData)

  export default {
    name: 'markets',
    components: {
      MarketItem
    },
    mounted () {
      let vm = this
      setInterval(() => {
        this.now = Math.trunc((new Date()).getTime() / 1000)
        if (new Date().getSeconds() === 0) {
          refreshMarkets(vm.markets)
        }
      }, 1000)
    },
    methods: {},
    data () {
      return {
        markets: marketData,
        showOpenMarkets: true,
        showClosedMarkets: true
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }

  .container {

    width: 85%;
  }
</style>
