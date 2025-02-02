<template>
  <div class="backdrop-blur w-screen h-screen">
    <div class="bg-gray-950 opacity-70 p-2 flex items-center">
      <span class="" @click="$emit('menu-change', 'overview')">
        <BIconArrowLeft class="text-xl rounded-full bg-gray-600" />
      </span>
    </div>
    <div class="p-8">
      <div
        v-for="(option, index) in options"
        :key="index"
        class="text-white mb-4"
      >
        <span class="text-2xl font-semibold">{{ option.title }}</span>
        <div class="grid grid-cols-2 gap-2 items-center mt-2 w-72">
          <button
            v-for="(field, index) in option.fields"
            :key="index"
            @click=""
            :data-active="props.store.settings.temp_unit"
            class="px-4 py-2 border border-gray-200 rounded hover:bg-gray-200 cursor-pointer data-[active='celsius']:bg-gray-100 data-[active='celsius']:text-black"
          >
            {{ field.text }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { BIconArrowLeft } from "bootstrap-icons-vue";
import { reactive } from "vue";

const props = defineProps({ store: Object });
const emits = defineEmits(["store-update", "menu-change"]);
const store = reactive({});

const options = [
  {
    title: "Temperature",
    fields: [
      { text: "Celsius", value: "temp_c" },
      { text: "Fahrenheit", value: "temp_f" },
    ],
  },
  {
    title: "Wind Speed",
    fields: [
      { text: "km/h", value: "temp_c" },
      { text: "m/s", value: "temp_f" },
    ],
  },
  {
    title: "Pressure",
    fields: [
      { text: "mb", value: "pressure_mb" },
      { text: "in", value: "pressure_in" },
    ],
  },
  {
    title: "Precipitation",
    fields: [
      { text: "mm", value: "precip_mm" },
      { text: "in", value: "precip_in" },
    ],
  },
  {
    title: "Distance",
    fields: [
      { text: "Kilometers", value: "vis_km" },
      { text: "Miles", value: "vis_miles" },
    ],
  },
];
</script>
