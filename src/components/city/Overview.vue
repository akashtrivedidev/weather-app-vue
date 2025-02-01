<template>
  <div class="">
    <Forecast :data="props.data" />
    <AirConditions />
  </div>
</template>
<script setup>
import Searchbar from "../Searchbar.vue";
import AirConditions from "./AirConditions.vue";
import Forecast from "./Forecast.vue";

const props = defineProps({ data: Object });
</script>
