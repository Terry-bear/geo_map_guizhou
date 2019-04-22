<template>
  <div id="app">
    <!-- <GeoMap /> -->
    <r-map/>
  </div>
</template>

<script>
// import GeoMap from './components/GeoMap.vue'
import rMap from './components/registerMap.vue'

export default {
  name: 'app',
  components: {
    // GeoMap,
    rMap
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
