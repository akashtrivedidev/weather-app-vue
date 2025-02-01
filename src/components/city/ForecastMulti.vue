<template>
  <h3 class="font-semibold">{{ forecasts.length }}-Day Forecast</h3>
  <div class="" v-for="(fc, index) in forecasts" :key="index">
    {{ Math.round(fc.day.maxtemp_c) }}/{{ Math.round(fc.day.mintemp_c) }}
  </div>
</template>
<script setup>
defineProps({ forecasts: Object });
</script>
