<script setup lang="ts">
import { ref } from "vue";
import { Location } from "./types.ts";
import LocationPicker from "./components/LocationPicker.vue"
import MapComponent from "./components/MapComponent.vue";

const selectedLocation = ref<Location | null>(null);
function handleClickLocation(location: Location): void {
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
    <MapComponent />
  </div>
</template>

<style scoped>

</style>
