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

  <div class="container">
    <WeatherInfoNextHours v-if="data" :data="data" />

    <WeatherInfoNextFiveDays v-if="data" :data="data" />
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
</style>
