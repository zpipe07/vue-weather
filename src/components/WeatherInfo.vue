<script setup lang="ts">
import { ref, watch } from 'vue'

type WeatherData = {
  cod: string
  message: number
  cntA: number
  list: Array<{
    dt: number
    main: {
      temp: number
      feels_like: number
      temp_min: number
      temp_max: number
      pressure: number
      sea_level: number
      grnd_level: number
      humidity: number
      temp_kf: number
    }
    weather: Array<{
      id: number
      main: string
      description: string
      icon: string
    }>
    clouds: {
      all: number
    }
    wind: {
      speed: number
      deg: number
      gust: number
    }
    visibility: number
    pop: number
    rain?: {
      '3h': number
    }
    snow?: {
      '3h': number
    }
    sys: {
      pod: 'n' | 'd'
    }
    dt_txt: string
  }>
  city: {
    id: number
    name: string
    coord: {
      lat: number
      lon: number
    }
    country: string
    population: number
    timezone: number
    sunrise: number
    sunset: number
  }
}

const API_KEY = '9170e0e85794088df319259526c55afd'

const props = defineProps<{
  city: string
}>()

const loading = ref(false)
const data = ref<WeatherData | null>(null)
const error = ref<string | null>(null)

watch(() => props.city, fetchData, { immediate: true })

async function fetchData(city: string) {
  error.value = data.value = null
  loading.value = true
  try {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=${API_KEY}&units=imperial`,
    )
    const weatherData = await response.json()
    data.value = weatherData
  } catch (err: unknown) {
    error.value = err?.toString() || 'Unknown error'
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div v-if="loading">Loading...</div>

  <div v-if="error">{{ error }}</div>

  <div v-if="data">
    <h2>Next hours</h2>
    <ul v-if="data" class="list">
      <li class="item" v-for="(item, index) in data.list" :key="index">
        <h4>{{ item.main.temp }}°F</h4>
        <h4>{{ item.pop * 100 }}%</h4>
        <img :src="'http://openweathermap.org/img/wn/' + item.weather[0].icon + '@2x.png'" />
        <h4>{{ new Date(item.dt * 1000).toLocaleTimeString() }}</h4>
        <h4>{{ new Date(item.dt * 1000).toDateString() }}</h4>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.list {
  padding: 0;
  margin: 0;
  list-style: none;
  display: flex;
  max-width: 100%;
  overflow-x: auto;
}
.item {
  list-style: none;
  margin: 1rem 0;
  padding: 1rem;
}
</style>
