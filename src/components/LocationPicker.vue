<script setup lang="ts">
  import { ref } from "vue";
  import { Location } from "../types.ts";
  import favoriteLocations from '../data';
  import LocationCard from './LocationCard.vue';

  const locationsData = ref(favoriteLocations);
  const selectedLocation = ref();

  function handleSelectedLocation(location: Location): void {
    selectedLocation.value = location;
  }

</script>

<template lang="">
  <div class="location-picker">
    <LocationCard
        v-for="location in locationsData"
        :key="location.id"
        :location="location"
        :selected="location === selectedLocation"
        @click="handleSelectedLocation(location)"
      />
  </div>
</template>


<style lang="">

</style>