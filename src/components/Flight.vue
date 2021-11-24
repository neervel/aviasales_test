<template>
  <div class="flight">
    <div class="flight-price">{{ flight.price | setSpace }} Р</div>
    <img
      :src="`//pics.avs.io/99/36/${flight.carrier}.png`"
      alt=""
      class="flight__img"
    />

    <div class="flight-item">
      <div class="flight-item__header">{{ flightTo.origin }} - {{ flightTo.destination }}</div>
      <div class="flight-item__info">{{ flightTime(flightTo.date, flightTo.duration) }}</div>
    </div>
    <div class="flight-item">
      <div class="flight-item__header">В ПУТИ</div>
      <div class="flight-item__info">{{ flightDuration(flightTo.duration) }}</div>
    </div>
    <div class="flight-item">
      <div class="flight-item__header">{{ stopCount(flightTo.stops) }}</div>
      <div class="flight-item__info">{{ flightTo.stops.join(', ') }}</div>
    </div>
    <div class="flight-item">
      <div class="flight-item__header">{{ flightFrom.origin }} - {{ flightFrom.destination }}</div>
      <div class="flight-item__info">{{ flightTime(flightFrom.date, flightFrom.duration) }}</div>
    </div>
    <div class="flight-item">
      <div class="flight-item__header">В ПУТИ</div>
      <div class="flight-item__info">{{ flightDuration(flightFrom.duration) }}</div>
    </div>
    <div class="flight-item">
      <div class="flight-item__header">{{ stopCount(flightFrom.stops) }}</div>
      <div class="flight-item__info">{{ flightFrom.stops.join(', ') }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Flight",
  props: {
    flight: {
      type: Object,
      default: () => ({}),
    },
  },
  computed: {
    flightTo() {
      return this.flight.segments[0]
    },
    flightFrom() {
      return this.flight.segments[1]
    },
    
  },
  filters: {
    setSpace: function (value) {
      if (!value) return "";
      value = value.toString();
      return value.replace(/\B(?=(\d{3})+(?!\d))/g, " ");
    },
  },
  methods: {
    stopCount(stop) {
      let stopCountStr = "";
      if (stop.length === 0) stopCountStr = "без пересадок";
      else if (stop.length === 1) stopCountStr = `${stop.length} пересадка`;
      else if (stop.length > 1 && stop.length < 5) stopCountStr = `${stop.length} пересадки`;
      else stopCountStr = `${stop.length} пересадок`;
      return stopCountStr;
    },
    flightDuration(time) {
      return `${parseInt(time/60)}ч ${time % 60}м`
    },
    flightTime(time, duration) {
      let depHours = time.split('T')[1].split(':')[0];
      let depMins = time.split('T')[1].split(':')[1];
      let durHours = parseInt (duration / 60)
      if (+durHours + depHours >= 24) durHours = (+durHours + depHours) % 24
      let durMins = duration % 60
      if (+durMins + depMins >= 60) {
        durHours += 1;
        durMins = (+durMins + depMins) % 60;
      }
      if (durHours.toString().length === 1) durHours = "0"+durHours
      if (durMins.toString().length === 1) durMins = "0"+durMins
      return `${depHours}:${depMins} - ${durHours}:${durMins}`
    }
  },
};
</script>

<style scoped>
.flight {
  width: 500px;
  border-radius: 5px;
  box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.1);
  background-color: #fff;
  padding: 20px;
  margin-bottom: 20px;
  display: grid;
  grid-gap: 20px;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-columns: 1fr 1fr 1fr;
}
.flight-price {
  color: #2196f3;
  font-size: 24px;
  grid-column: 1/3;
}
.flight__img {
  max-height: 36px;
  width: auto;
  margin: 0 auto;
}
.flight-item__header {
  font-size: 12px;
  line-height: 18px;
  color: #A0B0B9;
  text-transform: uppercase;
}
.flight-item__info {
  font-size: 14px;
  line-height: 21px;
}
</style>