<script setup lang="ts">
  import { ref } from "vue";
  import { Location } from "../types.ts";
  import favoriteLocations from '../data';
  import LocationCard from './LocationCard.vue';

  const locationsData = ref(favoriteLocations);
  const selectedLocation = ref();

  function handleSelectedLocation(location: Location): void {
    selectedLocation.value = location;
    console.log("Hello")
  }

</script>

<template>
  <div class="location-picker p-2 location-list d-flex flex-column h-100">
    <LocationCard
        v-for="location in locationsData"
        :key="location.id"
        :location="location"
        :selected="location === selectedLocation"
        @click="handleSelectedLocation(location)"
      />
  </div>
</template>


<style>
  .location-picker {
    position: absolute;
    top: 10%;
  }
</style>