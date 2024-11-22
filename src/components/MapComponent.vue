<script setup lang="ts">
import { onMounted } from "vue";
import 'ol/ol.css';
import Map from 'ol/Map';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import {OSM, Vector} from 'ol/source.js';
import Feature from 'ol/Feature';
import Point from 'ol/geom/Point';
import VectorLayer from 'ol/layer/Vector';
import {Style, Icon} from 'ol/style.js';
import { fromLonLat } from 'ol/proj';
import Select from "ol/interaction/Select"
import coordinates from "../../coordinates.ts"

onMounted(() => {
  initiateMap();
});

function initiateMap() {
  const map = new Map({
    target: 'map', // ID of the map container
    layers: [
      new TileLayer({
        source: new OSM(), // OpenStreetMap base layer
      }),
    ],
    view: new View({
      center: fromLonLat([13.4900, 52.4672]), // Coordinates for Berlin 1501659.13533162, 6885117.554255853
      zoom: 16,
    }),
  });

  createPinsFromList(map);
}

const vectorSource = new Vector();
// vector layer to display pins
const vectorLayer = new VectorLayer({
  source: vectorSource,
});

// create a pin
const pinStyle = new Style({
  image: new Icon({
    anchor: [0.5, 1],
    src: 'src/assets/location-pin-alt-svgrepo-com.svg',
    scale: 0.05,
  })
})

function createPinsFromList(map: Map) {
  map.addLayer(vectorLayer);
  coordinates.forEach((coord) => {
    const pin = new Feature({
      geometry: new Point(coord.longlat)
    })
    pin.setStyle(pinStyle);
    vectorSource.addFeature(pin);
    console.log('Pin added at:', coord.longlat);
  })
}

function selectLocationPin() {
  const selectedStyle = new Style({
    image: new Icon({
      anchor: [0.5, 1],
      src: 'src/assets/active-location-pin-alt-svgrepo-com.svg',
      scale: 0.05,
    })
  })
  const selection = new Select({
    style: selectedStyle,
  })
}

// function createPinOnClick(map: Map) {
//   // source to store pin data
//   map.addLayer(vectorLayer);
//   map.on("click", (event) => {
//     const coordinate = event.coordinate;
//     console.log(coordinate);
//     const pin = new Feature({
//       geometry: new Point(coordinate),
//     })
//     pin.setStyle(pinStyle);
//     vectorSource.addFeature(pin);
//     console.log('Pin added at:', coordinate);
//   })
// }

</script>

<template>
  <div id="map">
  </div>

</template>

<style></style>
