<script setup lang="ts">
  import { Location } from "../types.ts";
  import { ref } from "vue";

  defineProps<{ location: Location }>();
  const emit = defineEmits(["selected"])
  const selected = ref(false);
  function selectLocation() {
    selected.value = !selected.value;
    emit("selected", location.title)
  }

</script>

<template>
  <div class="list-group-item card mb-2" :class="{'active': selected}" @click="selected = !selected">
    <div class="card-body">
      <h5 class="card-title">{{ location.title }}</h5>
      <p class="card-text">{{ location.description }}</p>
      <p class="card-text">{{ location.address }} <br> {{ location.postalCode }}</p>
    </div>
  </div>
</template>


<style lang="">

</style>