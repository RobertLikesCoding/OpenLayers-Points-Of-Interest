<script setup lang="ts">
import LocationCard from './components/LocationCard.vue';
import favoriteLocations from './data';
import { ref } from "vue";

const locationsData = ref(favoriteLocations);
</script>

<template>
  <div class="container d-flex">
    <div class="list-group">
      <h2 class="pb-2">Baumschulenweg</h2>
      <LocationCard
        v-for="location in locationsData"
        :key="location.id"
        :location="location"
      />
    </div>
    <div class="map">

    </div>
  </div>
</template>

<style scoped></style>
