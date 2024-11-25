<script setup lang="ts">
import { onMounted, ref } from "vue";
import 'ol/ol.css';
import Map from 'ol/Map';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import { OSM, Vector } from 'ol/source.js';
import Feature from 'ol/Feature';
import Point from 'ol/geom/Point';
import VectorLayer from 'ol/layer/Vector';
import { Style, Icon } from 'ol/style.js';
import { fromLonLat } from 'ol/proj';
import Select from "ol/interaction/Select"
import coordinates from "../../locations-data.ts"
import type { LocationType } from "../../locations-data.ts"
import { watch } from "vue";

const map = ref<Map | null>(null);
const props = defineProps<{
  selectedLocation: LocationType | null,
}>();

watch(() => props.selectedLocation, (newLocation) => {
  if (newLocation) {
    highlightPin(newLocation.id); // Call a function to update the pin style
  }
});

onMounted(() => {
  initiateMap();
});

function initiateMap() {
  map.value = new Map({
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

  createPinsFromList();
  map.value.addInteraction(selectLocation);
}

const activePinStyle = new Style({
  image: new Icon({
    anchor: [0.5, 1],
    src: 'src/assets/active-locationmarker-svgrepo-com.svg',
    scale: 0.05,
  })
})

const selectLocation = new Select({
  style: activePinStyle,
});

const vectorSource = new Vector();
// vector layer to display pins
const vectorLayer = new VectorLayer({
  source: vectorSource,
});

// create a pin
const defaultPinStyle = new Style({
  image: new Icon({
    anchor: [0.5, 1],
    src: 'src/assets/locationmarker-svgrepo-com.svg',
    scale: 0.05,
  })
})

function createPinsFromList() {
  map.value?.addLayer(vectorLayer);
  coordinates.forEach((coord) => {
    const pin = new Feature({
      geometry: new Point(coord.coordinates),
      id: coord.id,
    })
    pin.setStyle(defaultPinStyle);
    vectorSource.addFeature(pin);
  })
}

function highlightPin(selectedId: number) {
  vectorSource.getFeatures().forEach((feature) => {
    // Reset all pins to default style
    feature.setStyle(defaultPinStyle);

    // Apply active style if the feature matches the selected ID
    if (feature.get("id") === selectedId) {
      feature.setStyle(activePinStyle);
    }
  });
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
//     pin.setStyle(defaultPinStyle);
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
