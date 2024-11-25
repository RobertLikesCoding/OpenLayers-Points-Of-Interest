<script setup lang="ts">
  import { LocationType } from "../../locations-data";

  defineProps<{
    location: LocationType,
    selected: boolean,
  }>();
</script>

<template>
  <div class="list-group-item card mb-3 p-0 location-card shadow" :class="{'border-success border-3': selected}">
    <div class="card-body">
      <h5 class="card-title">{{ location.name }}</h5>
      <p class="card-text">{{ location.description }}</p>
      <p class="card-text">{{ location.address }} <br> {{ location.postalCode }}</p>
    </div>
  </div>
</template>


<style>
  .location-card {
    z-index: 10;
    width: 250px;
  }
</style>