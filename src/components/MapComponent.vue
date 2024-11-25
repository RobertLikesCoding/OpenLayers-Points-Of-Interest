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
import locations from "../../locations-data.ts"
import type { LocationType } from "../../locations-data.ts"
import { watch } from "vue";

const map = ref<Map | null>(null);
const props = defineProps<{
  selectedLocation: LocationType | null,
  handleClickLocation: (location: LocationType) => void;
}>();


onMounted(() => {
  initiateMap();
  watch(() => props.selectedLocation, (newLocation) => {
    if (newLocation) {
      highlightPin(newLocation.id);
    }
  });
});

function initiateMap() {
  map.value = new Map({
    target: 'map',
    layers: [
      new TileLayer({
        source: new OSM(), // OpenStreetMap base layer
      }),
    ],
    view: new View({
      center: fromLonLat([13.4900, 52.4672]),
      zoom: 16,
    }),
  });

  pinClickHandler(map.value as Map);
  createPinsFromList();
}

const vectorSource = new Vector();
// vector layer to display pins
const vectorLayer = new VectorLayer({
  source: vectorSource,
});

const defaultPinStyle = new Style({
  image: new Icon({
    anchor: [0.5, 1],
    src: 'src/assets/locationmarker-svgrepo-com.svg',
    scale: 0.05,
  })
});

const activePinStyle = new Style({
  image: new Icon({
    anchor: [0.5, 1],
    src: 'src/assets/active-locationmarker-svgrepo-com.svg',
    scale: 0.05,
  })
});

function createPinsFromList() {
  map.value?.addLayer(vectorLayer);
  locations.forEach((location) => {
    const pin = new Feature({
      geometry: new Point(location.coordinates),
      id: location.id,
    })
    pin.setStyle(defaultPinStyle);
    vectorSource.addFeature(pin);
  })
}

function highlightPin(selectedId: number) {
  vectorSource.getFeatures().forEach((feature) => {
    // reset all pins to default style
    feature.setStyle(defaultPinStyle);
    // apply active style if the feature matches the selected ID
    if (feature.get("id") === selectedId) {
      feature.setStyle(activePinStyle);
    }
  });
}

function pinClickHandler(map: Map) {
  map.on("click", (event) => {
    map.forEachFeatureAtPixel(event.pixel, (feature) => {
      const featureId = feature.get('id');
      if (featureId !== undefined) {
        highlightPin(featureId);
        const searchLocation = locations.find(location => location.id === featureId)
        if (searchLocation) {
          props.handleClickLocation(searchLocation);
        }
      }
    });
  });
}
</script>

<template>
  <div id="map">
  </div>

</template>

<style></style>
