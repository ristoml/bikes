<template>
  <div id="app">   
    <BikeMap/>
  </div>
</template>

<script>
import BikeMap from './components/BikeMap.vue'

export default {
  name: 'App',
  components: {        
    BikeMap
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
