<script setup>
import { API_KEY, BASE_URL } from "@/constants/index";

const city = ref("");
const weatherInfo = ref(null);

const getWeatherData = () => {
  fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
    .then((res) => res.json())
    .then((data) => weatherInfo.value = data);
};

const isError = computed(() => weatherInfo.value?.cod !== 200);

</script>

<template>
  <div>
    <HEAD>
      <title>Weather App</title>
    </HEAD>

    <div class="page">
      <main class="main">
        <div class="container">
          <div class="laptop">
            <div class="sections">
              <section
                :class="[
                  'section',
                  'section-left',
                  { 'section-left-hidden': isError },
                ]"
              >
                <div class="info">
                  <div class="city-inner">
                    <input
                      @keyup.enter="getWeatherData"
                      v-model="city"
                      type="text"
                      class="search"
                      placeholder="Input a city"
                    />
                  </div>
                  <TheWeatherSummary
                    v-if="!isError"
                    :weatherInfo="weatherInfo"
                  />
                </div>
              </section>
              <section v-if="!isError" class="section section-right">
                <TheHighlights :weatherInfo="weatherInfo" />
              </section>
            </div>
            <div v-if="!isError" class="sections">
              <TheCoords :coord="weatherInfo?.coord" />
              <TheHumidity :humidity="weatherInfo?.main.humidity" />
            </div>
            <div v-else class="error">
              <div class="error-title">Something went wrong</div>
              <div v-if="weatherInfo?.message" class="error-message">
                {{ weatherInfo?.message }}
              </div>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<style scoped lang="sass">
@import '@/assets/styles/variables'
.page
  position: relative
  display: flex
  justify-content: center
  align-items: center
  min-height: 100vh
  padding: 20px 0
  background-color: #59585d
.laptop
  width: 100%
  padding: 20px
  background-color: #0e100f
  border-radius: 25px

.sections
  display: flex
  width: 100%

  @media (max-width: 767px)
    flex-direction: column

.section-left
  width: 30%
  padding-right: 10px

  &-hidden
    width: auto
    padding-right: 0px

  @media (max-width: 767px)
    width: 100%
    padding-right: 0

.section-right
  width: 70%
  padding-left: 10px

  @media (max-width: 767px)
    width: 100%
    margin-top: 16px
    padding-left: 0

.city-inner
  position: relative
  display: inline-block
  width: 100%

  &::after
    content: ''
    position: absolute
    top: 0
    right: 10px
    width: 25px
    height: 25px
    background: url('@/static/img/search.svg') no-repeat 50% 50%
    background-size: contain
    transform: translateY(50%)
    cursor: pointer

.info
  height: 100%
  padding: 16px
  background: url('@/static/img/gradient-1.jpg') no-repeat 50% 50%
  background-size: cover
  border-radius: 25px

.search
  width: 100%
  padding: 16px
  font-family: 'Inter', Arial, sans-serif
  color: $white
  background-color: rgba(0, 0, 0, 0.75)
  border-radius: 16px
  border: none
  outline: none
  cursor: pointer

.section-bottom
  width: 50%
  margin-top: 16px

  @media (max-width: 767px)
    width: 100%

.error
  text-align: center
  &-title
    margin: 10px 0
    font-size: 20px
    font-weight: bold
</style>
