<script setup lang="ts">
  import { Location } from "../types.ts";

  defineProps<{
    location: Location,
    selected: boolean,
  }>();
</script>

<template>
  <div class="list-group-item card mb-3 p-0 location-card shadow" :class="{'active': selected}">
    <div class="card-body">
      <h5 class="card-title">{{ location.title }}</h5>
      <p class="card-text">{{ location.description }}</p>
      <p class="card-text">{{ location.address }} <br> {{ location.postalCode }}</p>
    </div>
  </div>
</template>


<style>
  .location-card {
    z-index: 10;
    width: 250px;
    border: 2px solid black;
  }
</style>