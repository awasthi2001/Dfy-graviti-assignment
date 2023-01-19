
<template>
<div>
 <span>
        Add GeoJsonFile
    </span>
    <input @change="handleJsonFile"   type="file"/>
  <l-map style="height: 450px; width:450px; margin-left:350px " :zoom="zoom" :center="center" >
    <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
    <l-polygon :lat-lngs="polygon.latlngs" :color="polygon.color"></l-polygon>
  </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LPolygon } from "vue2-leaflet";

export default {
  components: {
    LMap,
    LTileLayer,
    LPolygon
  },
  data() {
    return {
      isFile : false,
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 10,
      center: [30,15],
      polygon: {
        latlngs: [
                [30, 10], [10, 10], [10, 0], [20, 40]

          ],
        color: "green",
      }
    };
  },
  methods : {
   handleJsonFile(e){
   var geojson = e.target.files[0];
   var read_JSON_File = new FileReader(); 
   read_JSON_File.onload = ()=>{
    var file_Data = JSON.parse(read_JSON_File.result);
    this.polygon.latlngs = file_Data.features[0].geometry.coordinates[0];
    this.center = file_Data.features[0].geometry.coordinates[0][0]
   }
   read_JSON_File.readAsText(geojson)
   }   
  }
};
</script>

<style>
</style>
