<script setup lang="ts">
  import { ref } from "vue";
  import { LocationType } from "../../locations-data";
  import favoriteLocations from '../../locations-data';
  import LocationCard from './LocationCard.vue';

  const locationsData = ref<LocationType[]>(favoriteLocations);
  const props = defineProps<{
    selectedLocation: LocationType | null;
    handleClickLocation: (location: LocationType) => void;
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
    overflow-y: scroll;
    scrollbar-color: rgb(219, 219, 219) rgb(255, 255, 255);
    scrollbar-width: thin;
  }
</style>