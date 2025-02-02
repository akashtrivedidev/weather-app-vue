<template>
  <div v-if="forecast" class="flex flex-col justify-between p-8 h-screen">
    <div class="flex justify-between w-full flex-grow">
      <div class="text-white">
        <h1 class="text-4xl font-semibold">
          {{ forecast.location.name }}
        </h1>
        <h2 class="text-lg">
          {{ forecast.location.region }}
        </h2>
        <h1 class="text-white text-5xl font-bold mt-4">
          {{ Math.round(forecast.current.temp_c) }} &deg;C
        </h1>
      </div>
      <div class="text-white flex flex-col gap-6">
        <div class="flex items-start gap-2">
          <BIconDropletHalf class="text-3xl" />
          <div class="">
            <div class="text-lg">Humidity</div>
            <div class="text-3xl text-white font-bold">
              {{ forecast.current.humidity }}%
            </div>
          </div>
        </div>
        <div class="flex items-start gap-2">
          <BIconArrowsCollapse class="text-3xl" />
          <div class="">
            <div class="text-lg">Air Pressure</div>
            <div class="text-3xl text-white font-bold">
              {{ forecast.current.pressure_in }}PS
            </div>
          </div>
        </div>
        <div class="flex items-start gap-2">
          <BIconCloudDrizzle class="text-3xl" />
          <div class="">
            <div class="text-lg">Chance of Rain</div>
            <div class="text-3xl text-white font-bold">
              {{ forecast.current.cloud }}%
            </div>
          </div>
        </div>
        <div class="flex items-start gap-2">
          <BIconWind class="text-3xl" />
          <div class="">
            <div class="text-lg">Wind Speed</div>
            <div class="text-3xl text-white font-bold">
              {{ forecast.current.wind_kph }} Km/h
            </div>
          </div>
        </div>
      </div>
    </div>

    <div :key="index" class="flex gap-4 px-4 overflow-auto">
      <div
        class="h-36 w-48 flex flex-col justify-between p-2 py-4 border border-gray-50 rounded backdrop-blur"
        v-for="(fr, index) in todayForecast()"
        :key="index"
      >
        <div class="text-white text-lg">
          {{
            new Date(fr.time).getHours() < 12
              ? (new Date(fr.time).getHours() % 12) + "AM"
              : (new Date(fr.time).getHours() % 12) + "PM"
          }}
        </div>
        <div class="text-white text-5xl font-medium">
          {{ Math.round(fr.temp_c) }} &deg; C
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import {
  BIconArrowsCollapse,
  BIconCloudDrizzle,
  BIconDropletHalf,
  BIconWind,
} from "bootstrap-icons-vue";

const props = defineProps({ forecast: Object });

function todayForecast() {
  const now = new Date().getHours();
  let all = props.forecast.forecast.forecastday[0].hour;
  // all = all.slice(now);
  return all;
}
</script>
