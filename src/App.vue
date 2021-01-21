<template>
  <div id="app">       
    <BikeStats/>
  </div>
</template>

<script>
import BikeStats from './components/BikeStats.vue'

export default {
  name: 'App',
  components: {    
    BikeStats
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
