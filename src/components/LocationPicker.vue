<script setup lang="ts">
  import { ref } from "vue";
  import { Location } from "../../locations-data";
  import favoriteLocations from '../../locations-data';
  import LocationCard from './LocationCard.vue';

  const locationsData = ref<Location[]>(favoriteLocations);
  const props = defineProps<{
    selectedLocation: Location | null;
    handleClickLocation: (location: Location) => void;
  }>();
</script>

<template>
  <div class="location-picker p-2 location-list d-flex flex-column h-100">
    <LocationCard
        v-for="location in locationsData"
        :key="location.id"
        :location="location"
        :selected="location === props.selectedLocation"
        @click="props.handleClickLocation(location)"
      />
  </div>
</template>


<style>
  .location-picker {
    position: absolute;
    top: 10%;
  }
</style>