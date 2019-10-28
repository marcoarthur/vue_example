<template>
  <div id="app">
	  <MapGeoJson/>
  </div>
</template>

<script>
import MapGeoJson from './components/MapGeoJson.vue'

export default {
  name: 'app',
  components: {
    MapGeoJson
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
