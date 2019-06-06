<template>
  <div id="app">
    <div class="header">
      <menuheader v-on:child-event="parentMethod"></menuheader>
    </div>
    <div class="header">
    </div>
    <div class="popcontents">
      <label for="pop-title">タイトル：</label>
      <input type="text" v-model="pop_title">
      <p>※タイトルを入力しないとポイントをプロットできません</p>
      <textarea cols="30" rows="5" readonly>{{ jsonArray }}</textarea>
    </div>
    <div id=map></div>
  </div>
</template>

<script>
import menuheader from './components/menuheader'
import 'leaflet/dist/leaflet.css'
import L from 'leaflet'
import uuidv4 from 'uuid/v4';

delete  L.Icon.Default.prototype._getIconUrl
L.Icon.Default.mergeOptions(
  {
    iconUrl: require( 'leaflet/dist/images/marker-icon.png' ),
    iconRetinaUrl: require( 'leaflet/dist/images/marker-icon-2x.png' ),
    shadowUrl: require( 'leaflet/dist/images/marker-shadow.png' )
  }
)

export default {
  data: function(){
    return {
      map: null,
      baseJson:{
        "title": "",
        "lat": "0.0",
        "lng": "0.0",
      },
      pop_title:"",
      jsonArray:[]
    }
  },
  mounted() {
      this.initMap();
  },
  components: {
    menuheader,
  },
  methods: {
    parentMethod (fromChildVal)
    {
      this.baseJson = fromChildVal;
    },
    initMap()
    {
      this.map = L.map('map', { center: L.latLng(35.6825, 139.752778), zoom: 15}).addLayer(
          L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png')
      ).on('click', p => 
      {
        if(this.pop_title === "") {
          alert('タイトルを設定してください');
          return;
        };
        const uuid = uuidv4();
        const marker = L.marker(p.latlng);
        marker.title = uuid;
        let json = JSON.parse(JSON.stringify(this.baseJson));
        json.title  = this.pop_title
        json.lat  = p.latlng.lat;
        json.lng  = p.latlng.lng;
        this.jsonArray.push(json);
        this.pop_title = "";
        marker.bindPopup(this.createElements(json));
        this.map.addLayer(marker);
      })
    },
    createElements(obj){
      let el = "";
      for(let k of Object.keys(obj)){
        el += `<div class="popupel"><span>${k}:</span><span>${obj[k]}</span></div>`;
      }
      return el;
    }
  }
}
</script>


<style>

html, body, #app{
  margin: 0;
  padding: 0;
}

#map{
  width: 90%;
  height: calc(100vh - 410px);
  margin: 10px auto;
  background: beige;
}

body { margin: 0; }

.header {
  width: 90%;
  margin: 20px auto;
}

.content > div{
  margin: 5px;
}

.popcontents{
  width: 90%;
  margin: 20px auto;
}

.popcontents > p {
  font-size: 0.6em;
  color: red;
}

.popcontents > div{
  margin-bottom: 5px;
}

.popupel {
  width: 140px;
}

.popupel > span {
  margin: 5px auto;
}
</style>