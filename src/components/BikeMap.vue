<template> 
<div style="height: 500px; width: 100%">
    <div style="height: 200px overflow: auto;">
      <h1>Citybikes in Uusimaa</h1>      
      <div id="weather">
        <img :src="icon" alt="description" style="width: 50px"><br>
        <strong>{{ weatherTemp }}&deg;C</strong><br>
        {{ weatherIcon.description }}
      </div>
    </div>
    <l-map      
      :zoom="zoom"
      :center="center"
      :options="mapOptions"
      style="height: 100%"
      @update:center="centerUpdate"
      @update:zoom="zoomUpdate"
    >
      <l-tile-layer
        :url="url"
        :attribution="attribution"
      />
      <l-marker v-for="item in bikelist" :key="item.message" :lat-lng="[item.latitude, item.longitude]">
        <l-popup>
          <div>
            Name: {{ item.name }}<br>
            Free bikes: {{ item.free_bikes }}<br>
            Free slots: {{ item.empty_slots }}
          </div>
        </l-popup>
      </l-marker>
      <l-routing-machine :waypoints="waypoints"/>      
    </l-map>
  </div>
</template>

<script>
import axios from "axios";
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LPopup } from "vue2-leaflet";
import { Icon } from "leaflet";
import LRoutingMachine from './LRoutingMachine.vue';


delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
});
export default {
  name: 'BikeMap',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LRoutingMachine    
  },
  props: {
    msg: String,    
  },
  data() {
      return {
        bikelist: [],
        weatherIcon: [],
        weatherTemp: [],
        icon: '',
        zoom: 14,
        center: latLng(60.1699, 24.9384),
        url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
        attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',                
        currentZoom: 11.5,
        currentCenter: latLng(47.41322, -1.219482),        
        mapOptions: {
          zoomSnap: 0.5
        },
        waypoints: [
        { lat: 60.1706244510591, lng: 24.941422068877223 },
        { lat: 60.156414040686236, lng: 24.921236982277836 }
      ] 
    };
  },
  mounted() {
    var self = this
    axios.get('http://api.citybik.es/v2/networks/citybikes-helsinki')
    .then(function (response) {           
      self.bikelist = response.data.network.stations
      console.log(self.bikelist)
    })
    .catch(function (error) {
      console.log(error)
    })
    axios.get('https://api.openweathermap.org/data/2.5/weather?id=658225&appid=// APP ID  \\&units=metric')
    .then(function (response) {           
      self.weatherIcon = response.data.weather[0]
      self.weatherTemp = Math.round(response.data.main.temp)      
      self.icon = 'http://openweathermap.org/img/wn/'+self.weatherIcon.icon+'@2x.png'      
      console.log(self.weatherIcon)
    })
    .catch(function (error) {
      console.log(error)
    })    
  },  
  methods: {
    stationInfo(itemid) {
    alert(itemid)
    },
    zoomUpdate(zoom) {
      this.currentZoom = zoom;
    },
    centerUpdate(center) {
      this.currentCenter = center;
    }
  }  
}
</script>

<style scoped>
h1 {
  margin: 0px 0 0;
  text-align: center;
}
#weather {
  text-align: right;
}
</style>