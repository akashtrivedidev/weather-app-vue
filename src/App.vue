<template>
  <div id="app-container" class="gap-4 h-screen mx-auto">
    <div class="" v-if="menu == 'overview'">
      <AppHeader
        v-on:location-change="(location) => (city = location)"
        v-on:menu-change="(new_menu) => (menu = new_menu)"
      />
      <Overview :forecast />
    </div>
    <div class="" v-else>
      <Settings
        :store
        @store-update="updateStore"
        v-on:menu-change="(new_menu) => (menu = new_menu)"
      />
    </div>
  </div>
</template>

<script setup>
import { onMounted, reactive, ref, watch } from "vue";
import AppHeader from "./components/AppHeader.vue";
import Overview from "./components/Overview.vue";
import Settings from "./components/Settings.vue";
import axios from "axios";

const menu = ref("overview");
const city = ref("kanpur");
const store = reactive({
  current_city: "kanpur",
  settings: {
    temp_unit: "celsius",
    speed_unit: "",
    pressure_unit: "",
    precipitation_unit: "",
    distance_unit: "",
  },
});
const forecast = ref(null);

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
onMounted(async () => {
  let fr = await getForecast(city.value);
  if (fr) {
    forecast.value = fr;
  } else {
    throw new Error();
  }
});
watch(city, async (current, old) => {
  let fr = await getForecast(city.value);
  if (fr) {
    forecast.value = fr;
  } else {
    throw new Error();
  }
});

onMounted(() => {
  let settings = localStorage.getItem("settings");
  if (settings) {
    settings = JSON.parse(settings);
    store.settings = settings;
  } else {
    localStorage.setItem("settings", JSON.stringify(store.settings));
  }
});
</script>

<style>
#app-container {
  background-image: url("../public/assets/img/background.jpg");
  background-position: center;
  background-size: cover;
}
</style>
