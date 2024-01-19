<template>
  <div id="map" tabindex="0" ></div>
  <div id="marker" title="Marker">
    <!-- <img src="https://cdn1.iconfinder.com/data/icons/Map-Markers-Icons-Demo-PNG/256/Map-Marker-Marker-Outside-Azure.png" style="width: 40px; height: 40px;"> -->
  </div>
</template>
<script setup>
import { onMounted, toRefs, ref } from 'vue';
import Map from 'ol/Map';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import Overlay from 'ol/Overlay';
import * as proj from 'ol/proj';
import * as interaction from 'ol/interaction';
import XYZ from 'ol/source/XYZ';
import Geocoder from "ol-geocoder";
import Popup from "ol-popup";

// Props & Emits
const props = defineProps({
  options: {
    default() {
      return {
        adaptive: false,
        position: {
          lat:16.3725,
          long:48.208889,
          zoom:2,
        },
        search: {
          show: true,
          placeholder: "Type location name...",

        },
        picker: {
          show: true,
          label: "Pick here",
          icon: "https://cdn1.iconfinder.com/data/icons/Map-Markers-Icons-Demo-PNG/256/Map-Marker-Marker-Outside-Azure.png"
        },
      }
    },
  }
});

const emits = defineEmits([
  "onChange",
  "onAfterInitialize",
])

// variables
const { options } = toRefs(props);
const addressData = ref({});
var popup = new Popup({
  panMapIfOutOfView: false
});

// Functions
const getMapData = (lat,lon) => {
  const toFix = proj.toLonLat([lon,lat])
  const url = `https://nominatim.openstreetmap.org/reverse?format=json&lat=${toFix[1]}&lon=${toFix[0]}&addressdetails=1&countrycodes=id`;
  fetch(url, {
    method: 'get', // Default is 'get'
  })
  .then(response => response.json())
  .then(json => {
    addressData.value = json;
    popup.show([lon,lat], `${json.display_name}<br><a style="color: navy; font-size: 10px;">${options.value.picker.label}</a>`);
    const result = {
      address_name: json.display_name,
      address_details: json,
      origin: {
        lat: lat,
        lon: lon
      },
      coordinates: {
        lat: json.lat,
        lon: json.lon
      }
    }

    getChange(result);
  });
}

const initializeMap = () => {
  var pos = proj.fromLonLat([options.value.position.long, options.value.position.lat]);
  let map = new Map({
    target: 'map',
    layers: [
      new TileLayer({
        source: new XYZ({
          url: 'https://tile.openstreetmap.org/{z}/{x}/{y}.png'
        })
      })
    ],
    view: new View({
      center: pos,
      zoom: 6,
    })
  });

  var marker_el = document.getElementById('marker');
  var marker = new Overlay({
    position: pos,
    positioning: 'center-center',
    element: marker_el,
    stopEvent: false,
    dragging: false
  });
  map.addOverlay(marker);

  var geocoder = new Geocoder('nominatim', {
    provider: 'photon',
    lang: 'en',
    placeholder: 'Search for ...',
    targetType: 'text-input',
    limit: 5,
    debug: true,
    autoComplete: true,
    keepOpen: true
  });

  map.addControl(geocoder);


  map.addOverlay(popup);

  var latlng = [16.3725, 48.208889];
  popup.show(pos, options.value.picker.label);

  //Listen when an address is chosen
  geocoder.on('addresschosen', function(evt){
    window.setTimeout(function () {
      popup.show(evt.coordinate, evt.address.formatted);
    }, 1000);
  });

  map.on('click', function(evt){
    popup.hide();
    getMapData(evt.coordinate[1],evt.coordinate[0]);
    marker.setPosition(evt.coordinate);
    map.getView().animate({center: [evt.coordinate[0], evt.coordinate[1]]} );
    // {zoom: 11}
    marker.set('dragging', false);
  });

  // drag action
  var dragPan;
  map.getInteractions().forEach(function(inter){
    if (inter instanceof interaction.DragPan) {
      dragPan = inter;
    }
  });

// drag pin
marker_el.addEventListener('mousedown', function(evt) {
  dragPan.setActive(false);
  marker.set('dragging', true);
  // console.info('start dragging');
});

map.on('pointermove', function(evt) {
	if (marker.get('dragging') === true) {
  	marker.setPosition(evt.coordinate);
    popup.hide()
  }
});

map.on('pointerup', function(evt) {
	if (marker.get('dragging') === true) {
    getMapData(evt.coordinate[1],evt.coordinate[0]);
    dragPan.setActive(true);
    marker.set('dragging', false);
    map.getView().animate({center: [evt.coordinate[0], evt.coordinate[1]]} );
  }
});
};

// Emit Function

// Change response event
const getChange = (obj) => {
  emits("onChange", obj);
};

// After initalize event
const afterInitialize = (obj) =>{
  emits("onAfterInitialize", obj)
};


onMounted(() => {
  // console.log(options.value.position);
  initializeMap();
})
</script>
<style>
@import url(https://fonts.googleapis.com/css?family=Open+Sans);
@import url(https://cdnjs.cloudflare.com/ajax/libs/ol3/3.20.0/ol.css);
html, body, #map{
    width:100%;
    height:100%;
    margin:0;
    overflow:hidden;
}
body {
    font: 1em/1.5 "Open Sans",Helvetica,Arial,sans-serif;
    color: #222;
    font-weight: 400;
}

#map{
    position:absolute;
    z-index:1;
    top:0;
    bottom:0;
    left:0;
}
.ol-control button{
    background-color: rgba(40, 40, 40, 0.8) !important;
}
.ol-control button:hover{
    background-color: rgba(40, 40, 40, 1) !important;
}

.ol-popup {
    position: absolute;
    min-width: 190px;
    background-color: white;
    -webkit-filter: drop-shadow(0 1px 4px rgba(0,0,0,0.2));
    filter: drop-shadow(0 1px 4px rgba(0,0,0,0.2));
    padding: 15px;
    border-radius: 10px;
    border: 1px solid #ccc;
    bottom: 33px;
    left: -111px;
    text-align: center;
}
.ol-popup:after, .ol-popup:before {
    top: 100%;
    border: solid transparent;
    content: " ";
    height: 0;
    width: 0;
    position: absolute;
    pointer-events: none;
}
.ol-popup:before {
    border-top-color: #cccccc;
    border-width: 11px;
    left: 50%;
    margin-left: -11px;
}
.ol-popup:after {
    border-top-color: white;
    border-width: 10px;
    left: 50%;
    margin-left: -10px;
}
.ol-popup-closer {
    text-decoration: none;
    position: absolute;
    top: 2px;
    right: 8px;
}
.ol-popup-closer:after {
    content: "✖";
}

.ol-control, .ol-scale-line {
  z-index: 10;
}

.gcd-txt-container .gcd-txt-result{
  margin: 0 auto;
  width: 42.4%;
  background: white;
  list-style: none;
  padding-left: 0;
  border-radius: 0 0 4px 4px;
  position: absolute;
  top: 55px;
  left: 0;
  right: 0;
  text-align: left;
}
.gcd-txt-container .gcd-txt-result li {
  padding: 10px;
  font-size: 12px;
}

.gcd-txt-container .gcd-txt-result li:hover{
  background: rgba(0,0,0,.1);
  cursor: pointer;
}

.gcd-txt-container .gcd-txt-result li a{
  text-decoration: none;
  color: #333;
}

.gcd-txt-control {
  position: absolute;
  text-align: center;
  left: 0;
  top: 0;
  width: 100%;
  padding: 10px 0;
}

.gcd-txt-control .gcd-txt-input {
  padding: 15px;
  width: 40%;
  border: none;
  border-radius: 4px;
  outline: none;
  font-size: 16px;
  background-color: white !important;
}

.gcd-txt-search {
  display: none;
}

#marker {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  background:url(https://cdn1.iconfinder.com/data/icons/Map-Markers-Icons-Demo-PNG/256/Map-Marker-Marker-Outside-Azure.png) no-repeat top center;
  background-size:100%;
  opacity: 1;
  cursor: move;
}
</style>
