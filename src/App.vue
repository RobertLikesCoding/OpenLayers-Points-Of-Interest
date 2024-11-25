<script setup lang="ts">
import { ref } from "vue";
import { LocationType } from "../locations-data";
import LocationPicker from "./components/LocationPicker.vue"
import MapComponent from "./components/MapComponent.vue";

const selectedLocation = ref<LocationType | null>(null);
function handleClickLocation(location: LocationType): void {
    selectedLocation.value = location;
  }
</script>

<template>
  <div class="d-flex w-100">
    <div class="list-group">
      <h2 class="p-2">Baumschulenweg</h2>
      <LocationPicker
        :selected-location="selectedLocation"
        :handle-click-location="handleClickLocation"
      />
    </div>
    <MapComponent :selected-location="selectedLocation" />
  </div>
</template>

<style scoped>

</style>
