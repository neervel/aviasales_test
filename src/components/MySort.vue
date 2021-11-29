<template>
  <div class="sort">
    <button class="sort-btn" :class="{active: sortType === 'cheap'}" @click="setSort('cheap')">Самый дешевый</button>
    <button class="sort-btn" :class="{active: sortType === 'fast'}" @click="setSort('fast')">Самый быстрый</button>
  </div>
</template>

<script>
export default {
  name: "MySort",
  data() {
    return{
      sortType: "cheap",
    }
  },
  methods: {
    setSort(arg) {
      this.sortType = arg;
      this.$emit("setSort", this.sortType)
    }
  },
}
</script>

<style scoped>
.sort {
  height: 100%;
  display: flex;
  border-radius: 5px;
  overflow: hidden;
}
.sort button {
  display: block;
  flex-basis: 50%;
  height: 100%;
  background-color: #fff;
  cursor: pointer;
  border: 1px solid #DFE5EC;
  font-size: 12px;
  text-transform: uppercase;
}
.sort button:first-child {
  border-radius: 5px 0 0 5px;
}
.sort button:last-child {
  border-radius: 0 5px 5px 0;
}
.sort button.active {
  color: #fff;
  background-color: #2196F3;
  border-color: #2196F3;
}
</style>