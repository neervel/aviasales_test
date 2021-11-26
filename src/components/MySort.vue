<template>
  <div class="sort">
    <button class="sort-btn active" @click="setSort('cheap')">Самый дешевый</button>
    <button class="sort-btn" @click="setSort('fast')">Самый быстрый</button>
  </div>
</template>

<script>
export default {
  name: "MySort",
  methods: {
    setSort(arg) {
      const btnList = document.getElementsByClassName("sort-btn")
      for (var i = 0; i < btnList.length; i++) {
        document.getElementsByClassName("sort-btn")[i].classList.toggle("active")
      }
      this.$emit("setSort", arg)
    }
  }
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