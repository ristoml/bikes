<template>
  <div class="bikes">
    <h1>City bikes in Uusimaa</h1>
    <ul id="bikelist">
      <li v-for="item in bikelist.network.stations" :key="item.message">        
        Asema: {{ item.name }} - 
        vapaita paikkoja: {{ item.empty_slots }} - 
        vapaita pyöriä: {{ item.free_bikes }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: 'BikeStats',
  props: {
    msg: String
  },
  data() {
      return {
        bikelist: []        
      }
  },
  mounted() {
    var self = this
    axios.get('http://api.citybik.es/v2/networks/citybikes-helsinki')
    .then(function (response) {
      //console.log(response)
      self.bikelist = response.data
      console.log(self.bikelist)
    })
    .catch(function (error) {
      console.log(error)
    });
  } 
}
</script>

<style scoped>
h1 {
  margin: 40px 0 0;
  text-align: center;
}
ul {
  list-style-type: disc;
  padding: 10;
}
li {
  display: table-grid;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
