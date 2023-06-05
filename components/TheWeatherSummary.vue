<script setup>
const props = defineProps({
  weatherInfo: {
    type: [Object, null],
    required: true,
  },
});

const getCurrentDate = () =>
  new Date().toLocaleString("en-En", {
    weekday: "short",
    year: "numeric",
    month: "long",
    day: "numeric",
  });

const capitalizeFirstLetter = (text) => text[0].toUpperCase() + text.slice(1);

const getImageUrl = (text) => {
  const url = `../public/${text}.png`;
  return new URL(url, import.meta.url).href;
};
</script>

<template>
  <div v-if="weatherInfo" class="summary">
    <div class="pic-main">
      <img :src="getImageUrl(weatherInfo?.weather[0].description)" />
    </div>
    <div class="weather">
      <div class="temp">{{ Math.ceil(weatherInfo?.main.temp) }} °C</div>
      <div class="weather-desc text-block">
        {{ capitalizeFirstLetter(weatherInfo?.weather[0].description) }}
      </div>
    </div>
    <div class="city text-block">
      {{ weatherInfo.name }} , {{ weatherInfo?.sys.country }}
    </div>
    <div class="date text-block">{{ getCurrentDate() }}</div>
  </div>
</template>

<style lang="sass" scoped>
@import '@/assets/styles/variables'
@import '@/assets/styles/weatherSummary'
</style>
