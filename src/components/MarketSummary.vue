<template>
  <div class="row">
    <div class="card">
      <div class="card-header bell-countdown-header">
      <strong>  {{bellType}} NYSE/NASDAQ Countdown</strong>
      </div><br/>
      <div class="card-block clock" :id="market.id">
      </div>
    </div>
  </div>

</template>

<script>
  import moment from 'moment-timezone'
  import shared from '../shared'

  function addDaysIfWeekend (isoWeekday) {
    return isoWeekday === 6 ? 2 : isoWeekday === 7 ? 1 : 0
  }

  function countdownToOpeningBell (market) {
    const place = moment.tz(market.tz)
    const opensTime = moment(market.open, 'HH:mm')

    let addedDays = addDaysIfWeekend(place.isoWeekday())
    // If it only opens the day after, we add a day
    if (opensTime.hour() < place.hour()) {
      addedDays += 1
    }

    const opensInSeconds = (place.clone()
        .day(place.day() + addedDays)
        .hour(opensTime.hour())
        .minute(opensTime.minute())
        .second(0) - place) / 1000

    window.$('.clock#' + market.id).FlipClock(opensInSeconds, {
      clockFace: 'HourlyCounter', countdown: true
    })
  }

  function countdownToClosingBell (market) {
    const place = moment.tz(market.tz)
    const closingTime = moment(market.close, 'HH:mm')

    const closesInSeconds = (place.clone()
        .hour(closingTime.hour())
        .minute(closingTime.minute())
        .second(0) - place) / 1000

    window.$('.clock#' + market.id).FlipClock(closesInSeconds, {
      clockFace: 'HourlyCounter', countdown: true
    })
  }

  export default {
    props: ['market'],
    mounted () {
      if (shared.isMarkedOpen(this.market)) {
        this.bellType = 'Closing'
        countdownToClosingBell(this.market)
      } else {
        this.bellType = 'Opening'
        countdownToOpeningBell(this.market)
      }
    },
    data () {
      return {
        shared: shared,
        timeZone: moment.tz(shared.clientTz).zoneAbbr(),
        bellType: null
      }
    }
  }
</script>

<style scoped>
  .market-overview {
    margin-top: 8px;
  }

  .overview-row {
    margin-bottom: -5px;
  }

  .bell-countdown-header {
    padding: 0px 0px 0px 5px;
    font-size: 13px;
    background: #dadada;
  }

  .clock {
    padding: 8px;
  }
</style>
