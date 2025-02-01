<template>
  <div id="app-container" class="gap-4 h-screen mx-auto">
    <div class="bg-gray-950 opacity-70 py-2">
      <Searchbar
        class="mx-auto"
        @location-select="(location) => (city = location)"
      />
    </div>
    <div v-if="forecast" class="p-4 flex justify-between w-full">
      <div class="">
        <h1 class="text-white text-4xl font-semibold">
          {{ forecast.location.name }}
        </h1>
        <h2 class="text-gray-300 text-lg">
          {{ forecast.location.region }}
        </h2>
        <h1 class="text-white text-5xl font-bold">
          {{ Math.round(forecast.current.temp_c) }} &deg;C
        </h1>
      </div>

      <div class="">
        <div class="">
          <div class="text-gray-300 text-lg">Humidity</div>
          <div class="text-3xl text-white font-bold">
            {{ forecast.current.humidity }}%
          </div>
        </div>
        <div class="">
          <div class="text-gray-300 text-lg">Air Pressure</div>
          <div class="text-3xl text-white font-bold">
            {{ forecast.current.humidity }}PS
          </div>
        </div>
        <div class="">
          <div class="text-gray-300 text-lg">Chance of Rain</div>
          <div class="text-3xl text-white font-bold">
            {{ forecast.current.humidity }}%
          </div>
        </div>
        <div class="">
          <div class="text-gray-300 text-lg">
            <span class=""> </span>
            Wind Speed
          </div>
          <div class="text-3xl text-white font-bold">
            {{ forecast.current.humidity }} Km/h
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, reactive, ref, watch } from "vue";
import axios from "axios";
import Searchbar from "./components/Searchbar.vue";

const city = ref(null);
const forecast = ref(null);
const settings = reactive({});

async function getForecast(city) {
  const base = "http://api.weatherapi.com";
  const query_params = {
    key: import.meta.env.VITE_API_KEY,
    days: 3,
  };
  let url = new URL("/v1/forecast.json", base);
  url.searchParams.set("key", query_params.key);
  url.searchParams.set("q", city);
  url.searchParams.set("days", query_params.days);
  let response = await axios.get(url);
  if (response.status == 200) {
    return response.data;
  }
  return null;
}

watch(city, async (current, old) => {
  let fr = await getForecast(current);
  if (fr) {
    forecast.value = fr;
  } else {
    throw new Error();
  }
});

onMounted(async () => {
  let fr = await getForecast(city.value);
  if (fr) {
    forecast.value = fr;
  } else {
    throw new Error();
  }
});
</script>

<style>
#app-container {
  background-image: url("../public/assets/img/summer-fog.jpg");
  background-position: center;
  background-size: cover;
}
</style>
