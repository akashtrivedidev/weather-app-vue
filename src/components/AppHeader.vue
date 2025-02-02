<template>
  <div class="bg-gray-950 opacity-50 py-2 px-8 flex items-center">
    <Searchbar
      class="mx-auto"
      @location-select="(location) => $emit('location-change', location)"
    />
    <ul class="text-white text-2xl flex gap-4">
      <li class="cursor-pointer" @click="$emit('menu-change', 'settings')">
        <BIconGear class="" />
      </li>
    </ul>
  </div>
</template>
<script setup>
import { BIconGear } from "bootstrap-icons-vue";
import Searchbar from "./Searchbar.vue";

const emits = defineEmits(["location-change", "menu-change"]);
</script>
