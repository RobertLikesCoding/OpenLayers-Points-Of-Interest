<script setup>
import LocationPicker from "./components/LocationPicker.vue"
import MapComponent from "./components/MapComponent.vue";

</script>

<template>
  <div class="d-flex w-100">
    <div class="list-group">
      <h2 class="p-2">Baumschulenweg</h2>
      <LocationPicker />
    </div>
    <MapComponent />
  </div>
</template>

<style scoped>

</style>
