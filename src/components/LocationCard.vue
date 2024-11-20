<script setup lang="ts">
  import { Location } from "../types.ts";

  defineProps<{
    location: Location,
    selected: {type: boolean, default: false}
  }>();
</script>

<template>
  <div class="list-group-item card mb-2" :class="{'active': selected}">
    <div class="card-body">
      <h5 class="card-title">{{ location.title }}</h5>
      <p class="card-text">{{ location.description }}</p>
      <p class="card-text">{{ location.address }} <br> {{ location.postalCode }}</p>
    </div>
  </div>
</template>


<style>

</style>