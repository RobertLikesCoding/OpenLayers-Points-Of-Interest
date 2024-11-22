<script setup lang="ts">
import { onMounted, ref } from "vue";
import 'ol/ol.css';
import Map from 'ol/Map';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import OSM from 'ol/source/OSM';
import Feature from 'ol/Feature';
import Point from 'ol/geom/Point';
import VectorSource from 'ol/source/Vector';
import VectorLayer from 'ol/layer/Vector';
import Style from 'ol/style/Style';
import Icon from 'ol/style/Icon';
import { fromLonLat } from 'ol/proj';
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

const vectorSource = new VectorSource();
// vector layer to display pins
const vectorLayer = new VectorLayer({
  source: vectorSource,
});

// create a pin
const pinStyle = new Style({
  image: new Icon({
    anchor: [0.5, 1],
    src: 'https://cdn-icons-png.flaticon.com/512/684/684908.png', // Marker icon
    scale: 0.1,
  })
})

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
</script>

<template>
  <div id="map">
  </div>

</template>

<style></style>
