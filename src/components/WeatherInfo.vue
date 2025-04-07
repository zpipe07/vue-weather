<script setup lang="ts">
import { ref, watch } from 'vue'
import type { WeatherData } from '../types'
import WeatherInfoNextHours from '../components/WeatherInfoNextHours.vue'
import WeatherInfoNextFiveDays from '../components/WeatherInfoNextFiveDays.vue'

const API_KEY = '9170e0e85794088df319259526c55afd'

const props = defineProps<{
  city: string
}>()

const loading = ref(false)
const data = ref<WeatherData | null>(null)
const error = ref<string | null>(null)

watch(() => props.city, fetchWeatherData, { immediate: true })

const refreshWeatherData = () => {
  if (props.city) {
    fetchWeatherData(props.city)
  }
}

async function fetchWeatherData(city: string) {
  error.value = data.value = null
  loading.value = true
  try {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=${API_KEY}&units=imperial`,
    )

    if (!response.ok) {
      throw new Error(`${response.status} ${response.statusText}`)
    }

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
  <div v-if="error" class="error">
    <h3>Oops! There was an error</h3>
    <p>{{ error }}</p>
  </div>

  <div v-if="!error" class="container">
    <button class="refresh" @click="refreshWeatherData">Refresh</button>

    <WeatherInfoNextHours :data="data" :is-loading="loading" />

    <WeatherInfoNextFiveDays :data="data" :is-loading="loading" />
  </div>
</template>

<style scoped>
.error {
  background-color: #f8d7da;
  color: #721c24;
  padding: 1rem;
  border-radius: 0.5rem;
}
.error h4 {
  font-size: 1.25rem;
}
.refresh {
  align-self: flex-end;
  background-color: #007bff;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  cursor: pointer;
}
.refresh:hover {
  background-color: #0056b3;
}
.refresh:focus {
  outline: none;
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.5);
}
.container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
</style>
