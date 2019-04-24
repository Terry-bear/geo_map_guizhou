<template>
  <div id="app">
    <!-- <GeoMap /> -->
    <!-- <r-map/> -->
    <!-- <three-map id="container"/> -->
    <shebao-map/>
  </div>
</template>

<script>
// import GeoMap from './components/GeoMap.vue'
// import rMap from './components/registerMap.vue'
// import threeMap from './components/3Dmap.vue'
import shebaoMap from './components/shebaoMap.vue'

export default {
  name: 'app',
  components: {
    // GeoMap,
    // rMap
    // threeMap
    shebaoMap
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

#container{
  display: inline-block;
}
</style>
