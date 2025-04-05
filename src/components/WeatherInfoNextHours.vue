<script setup lang="ts">
import type { WeatherData } from '../types'

defineProps<{
  data: WeatherData | null
  isLoading: boolean
}>()
</script>

<template>
  <div v-if="isLoading" class="card loading" />

  <div v-if="data" class="card">
    <h2 class="heading">Next hours</h2>
    <ul v-if="data" class="list">
      <li class="item" v-for="(item, index) in data.list" :key="index">
        <h4>{{ item.main.temp.toFixed(0) }}°</h4>
        <h4>{{ item.pop * 100 }}%</h4>
        <img
          :src="'http://openweathermap.org/img/wn/' + item.weather[0].icon + '@2x.png'"
          width="75"
          height="75"
        />
        <h4>{{ new Date(item.dt * 1000).toLocaleTimeString() }}</h4>
        <h4>{{ new Date(item.dt * 1000).toDateString() }}</h4>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.card {
  border-radius: 0.25rem;
  border: 1px solid var(--color-border);
}
.card.loading {
  animation: pulse 1.5s infinite;
  min-height: 23rem;
}
@keyframes pulse {
  0%,
  100% {
    background-color: rgba(0, 0, 0, 0.025);
  }
  50% {
    background-color: rgba(0, 0, 0, 0.075);
  }
}
.heading {
  border-bottom: 1px solid var(--color-border);
  padding: 1rem;
}
.list {
  padding: 0 1rem;
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
  border-right: 1px solid var(--color-border);
  text-align: center;
}
.item:last-child {
  border-right: none;
}
</style>
