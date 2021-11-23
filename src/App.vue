<template>
  <div id="app">
    <Flight v-for="(flight, i) in flights" :flight="flight" :key="`${flight.price}-${i}`"/>
  </div>
</template>

<script>
import Flight from "./components/Flight.vue";
import axios from "axios";

export default {
  name: 'App',
  components: {
    Flight
  },
  data() {
    return {
      flights: []
    }
  },
  methods: {
    getFlights() {
      axios.get("https://front-test.beta.aviasales.ru/tickets?searchId=3y1ud")
        .then(response => this.flights = response.data.tickets)
        .catch(error => console.error(error))
    }
  },
  mounted() {
    this.getFlights();
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;600&display=swap');
* {
  font-family: "Open Sans", sans-serif;
  font-weight: 600;
  color: #4A4A4A;
}
body {
  margin: 0;
  background-color: #F3F7FA;
}
</style>
