<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
  import shared from './shared'
  import moment from 'moment-timezone'
  export default {
    name: 'app',
    mounted () {
      this.initClockTimer()
    },
    methods: {
      initClockTimer: function () {
        let vm = this
        var updateTime = function () {
          let localTimeZone = moment.tz(shared.clientTz)
          let tzAbbr = localTimeZone.zoneAbbr()
          let curTime = localTimeZone.format('HH:mm:ss')
          vm.localTimeDisplayed = curTime + ' ' + tzAbbr
        }
        updateTime()
        setInterval(updateTime, 1000)
      }
    },
    data () {
      return {
        localTimeDisplayed: null
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 0px;

  }

  #app > nav {
    margin-bottom: 0px;
  }

  .dark-bg {

  }

  #your-time {
    background-color: #000000;
    height: 5px;
    margin-top: 8px;
    padding-top: 8px;
    padding-left: 8px;
    padding-right: 8px;
    padding-bottom: 6px;
    color: #ffffff;
    font-weight: bold;
  }
</style>
