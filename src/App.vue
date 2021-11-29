<template>
  <div id="app">
    <img src="./assets/logo.svg" class="logo">
    <div class="search">
      <div class="filter">
        <MyFilter />
      </div>
      <div class="sort-block">
        <MySort @setSort="setSort"/>
      </div>
      <div class="search-loading" v-if="shownFlights.length === 0">
        Loading...
      </div>
      <div class="flights" v-else>
        <Flight v-for="(flight, i) in shownFlights" :flight="flight" :key="`${flight.price}-${i}`"/>
        <button class="flights-btn" @click="getMore">Показать еще 5 билетов!</button>
      </div>
    </div>
  </div>
</template>

<script>
import Flight from "./components/Flight.vue";
import MyFilter from "./components/MyFilter.vue";
import MySort from "./components/MySort.vue";
import axios from "axios";
import _ from "lodash";


export default {
  name: 'App',
  components: {
    Flight,
    MyFilter,
    MySort,
  },
  data() {
    return {
      flights: [],
      shownFlights: [],
      filters: [],
      sort: "cheap",
      currentPage: 0,
    }
  },
  methods: {
    getFlights() {
      let searchId = "";
      axios.get("https://front-test.beta.aviasales.ru/search")
        .then(response => {
          searchId = response.data.searchId
          axios.get(`https://front-test.beta.aviasales.ru/tickets?searchId=${searchId}`)
            .then(response => {
              this.flights = response.data.tickets;
              for (let i = 0; i < 5; i++) {
                this.shownFlights.push(this.flights[i]);
              }
              this.sortByPrice();
              this.currentPage ++;
            })
            .catch(error => console.error(error))
        }); 
    },
    sortByPrice() {
      this.shownFlights = _.sortBy(this.shownFlights, [function(item) { return item.price; }]);
    },
    sortByTime() {
      this.shownFlights = _.sortBy(this.shownFlights, function(item) { return +item.segments[1].duration + item.segments[0].duration})
    },
    getMore() {
      for (let i = this.currentPage * 5; i < this.currentPage * 5 + 5; i++) {
        this.shownFlights.push(this.flights[i]);
      }
      this.currentPage ++;
      if (this.sort === "cheap") this.sortByPrice();
      else if (this.sort === "fast") this.sortByTime();
    },
    setSort(arg) {
      if (arg === "cheap") {
        this.sortByPrice();
        this.sort = "cheap"
      } else if (arg === "fast") {
        this.sortByTime();
        this.sort = "fast";
      }
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
  box-sizing: border-box;
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
  grid-template-rows: 50px auto;
  margin: 50px auto;
  grid-gap: 20px;
  justify-content: center;
}
.search-loading {
  grid-column: 2;
}

.flights {
  grid-column: 2;
}
.flights-btn {
  display: block;
  margin: 0 auto;
  width: 100%;
  color: #fff;
  background-color: #2196F3;
  border: none;
  border-radius: 5px;
  font-size: 12px;
  text-transform: uppercase;
  height: 50px;
  letter-spacing: 0.5;
  cursor: pointer;
}
</style>
