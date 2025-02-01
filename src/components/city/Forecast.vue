<template>
  <h2 class="text-4xl font-medium text-white">{{ data.location.city }}</h2>
  <p class="font-medium text-gray-800 text-xl">
    {{ data.current.temperature }}
  </p>
</template>

<script setup>
defineProps({ data: Object });
</script>
