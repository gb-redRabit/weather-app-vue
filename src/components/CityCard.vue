<template>
  <div class="flex justify-between items-center py-6 px-3 bg-weather-secondary rounded-md shadow-md cursor-pointer ">

    <h2 class="text-3xl">{{ city.city }}</h2>



    <div class="flex flex-col gap-2">
      <p class="text-3xl self-end">
        {{ Math.round((city.weather.main.temp - 32) * (5 / 9)) }}&degC
      </p>
      <div class="flex gap-2">
        <span class="text-xs">
          Max:
          {{ Math.round((city.weather.main.temp_max - 32) * (5 / 9)) }}&degC
        </span>
        <span class="text-xs">
          Min:
          {{ Math.round((city.weather.main.temp_min - 32) * (5 / 9)) }}&degC
        </span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  city: {
    type: Object,
    default: () => { },
  },
});
</script>
