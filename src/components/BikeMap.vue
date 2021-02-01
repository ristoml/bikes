<template> 
<div style="height: 500px; width: 100%">
    <div style="height: 200px overflow: auto;">
      <h1>Citybikes in Uusimaa</h1>
      <p>Click on a marker to see more information</p> 
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
    </l-map>
  </div>
</template>

<script>
import axios from "axios";
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LPopup } from "vue2-leaflet";
import { Icon } from 'leaflet';

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
    LPopup    
  },
  props: {
    msg: String,    
  },
  data() {
      return {
        bikelist: [],
        zoom: 14,
        center: latLng(60.1699, 24.9384),
        url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
        attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',                
        currentZoom: 11.5,
        currentCenter: latLng(47.41322, -1.219482),
        showParagraph: false,
        mapOptions: {
          zoomSnap: 0.5
        },
        showMap: true        
    };
  },
  mounted() {
    var self = this
    axios.get('http://api.citybik.es/v2/networks/citybikes-helsinki')
    .then(function (response) {
      //console.log(response)
      self.bikelist = response.data.network.stations
      console.log(self.bikelist)
    })
    .catch(function (error) {
      console.log(error)
    });    
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
  margin: 40px 0 0;
  text-align: center;
}
ul {
  list-style-type: none;
  padding: 10;
}
li {
  display: table-grid;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.dropbtn {
  background-color: #4CAF50;
  color: white;
  padding: 16px;
  font-size: 16px;
  border: none;
}

.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f1f1f1;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
  z-index: 1;
}

.dropdown-content a {
  color: black;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
}

.dropdown-content a:hover {background-color: #ddd;}

.dropdown:hover .dropdown-content {display: block;}

.dropdown:hover .dropbtn {background-color: #3e8e41;}

</style>
