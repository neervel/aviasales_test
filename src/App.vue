<template>
  <div id="app">
    <img src="./assets/logo.svg" class="logo">
    <div class="search">
      <div class="filter">
        <MyFilter />
      </div>
      <div class="search-loading" v-if="flights.length === 0">
        Loading...
      </div>
      <div class="flights" v-else>
        <Flight v-for="(flight, i) in flights" :flight="flight" :key="`${flight.price}-${i}`"/>
      </div>
    </div>
  </div>
</template>

<script>
import Flight from "./components/Flight.vue";
import MyFilter from "./components/MyFilter.vue";
import axios from "axios";

export default {
  name: 'App',
  components: {
    Flight,
    MyFilter,
  },
  data() {
    return {
      flights: []
    }
  },
  methods: {
    getFlights() {
      let searchId = "";
      axios.get("https://front-test.beta.aviasales.ru/search")
        .then(response => {
          searchId = response.data.searchId
          axios.get(`https://front-test.beta.aviasales.ru/tickets?searchId=${searchId}`)
            .then(response => this.flights = response.data.tickets)
            .catch(error => console.log(error))
        })
        
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
.logo {
  display: block;
  margin: 0 auto;
  margin-top: 50px;
  height: 100px;
  width: 100px;
}
.search {
  display: grid;
  grid-template-columns: 230px 500px;
  margin: 50px auto;
  grid-gap: 20px;
  justify-content: center;
}
.flights {
  grid-column: 2;
}
</style>
