<template>
  <div class="max-w-xl relative">
    <input
      type="search"
      name="search"
      id="search-bar"
      v-model="query"
      class="w-full p-2 border border-gray-500 rounded bg-gray-600 text-black"
      placeholder="search"
    />
    <div
      v-if="locations != null"
      class="bg-white shadow absolute mt-1 w-full rounded"
    >
      <ul class="">
        <li
          @click="$emit('location-select', location.name)"
          class="text-black p-1 hover:bg-gray-100 cursor-pointer"
          v-for="(location, index) in locations"
          :key="index"
        >
          {{ location.name }}, {{ location.region }}, ({{ location.country }})
        </li>
      </ul>
    </div>
  </div>
</template>
<script setup>
import axios from "axios";
import { ref, watch } from "vue";

const query = ref("");
const locations = ref(null);
const emits = defineEmits(["location-select"]);

watch(query, async (current, old) => {
  if (current) {
    let data = await getForecast(current);
    locations.value = data;
  } else locations.value = null;
});

async function getForecast(current) {
  const base = "http://api.weatherapi.com";
  const query_params = {
    key: import.meta.env.VITE_API_KEY,
    q: current,
  };
  let url = new URL("/v1/search.json", base);
  url.searchParams.set("key", query_params.key);
  url.searchParams.set("q", current);
  let response = await axios.get(url);
  if (response.status == 200) {
    return response.data;
  }
  return null;
}
</script>
