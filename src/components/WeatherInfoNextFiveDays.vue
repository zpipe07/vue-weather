<script setup lang="ts">
import { computed } from 'vue'
import type { WeatherData, WeatherListItem } from '../types'

const props = defineProps<{
  data: WeatherData | null
  isLoading: boolean
}>()

const nextFiveDays = computed(() =>
  props.data?.list.reduce(
    (
      acc: Record<
        string,
        { high: number; low: number; icon: string; description: string; day: string }
      >,
      item: WeatherListItem,
    ) => {
      const key = item.dt_txt.split(' ')[0]
      const dateOptions: Intl.DateTimeFormatOptions = {
        weekday: 'short',
        month: 'short',
        day: 'numeric',
      }
      if (!acc[key]) {
        acc[key] = {
          high: item.main.temp_max,
          low: item.main.temp_min,
          icon: item.weather[0].icon,
          description: item.weather[0].description,
          day: new Date(item.dt_txt).toLocaleDateString('en-US', dateOptions),
        }
      } else {
        acc[key].high = Math.max(acc[key].high, item.main.temp_max)
        acc[key].low = Math.min(acc[key].low, item.main.temp_min)
      }
      return acc
    },
    {},
  ),
)
</script>

<template>
  <div v-if="isLoading" class="card loading" />

  <div v-if="data" class="card">
    <h2 class="heading">Next 5 days</h2>
    <ul v-if="data" class="list">
      <li class="item" v-for="(item, key) in nextFiveDays" :key="key">
        <img
          :src="'http://openweathermap.org/img/wn/' + item.icon + '@2x.png'"
          alt="Weather icon"
          width="75"
          height="75"
        />
        <div class="main">
          <h3>{{ item.day }}</h3>
          <h4 class="description">{{ item.description }}</h4>
        </div>
        <div class="temps">
          <h4>{{ item.high.toFixed(0) }}°</h4>
          <h4>{{ item.low.toFixed(0) }}°</h4>
        </div>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.card {
  border-radius: 0.25rem;
  border: 1px solid var(--color-border);
  width: 100%;
  max-width: 28rem;
  margin: 0 auto;
  background-color: var(--color-background);
}
.card.loading {
  animation: pulse 1.5s infinite;
  min-height: 23rem;
}
@keyframes pulse {
  0%,
  100% {
    background-color: rgba(255, 255, 255, 0.4);
  }
  50% {
    background-color: rgba(255, 255, 255, 0.6);
  }
}
.heading {
  border-bottom: 1px solid var(--color-border);
  padding: 1rem;
}
.list {
  padding: 0;
  margin: 0;
}
.item {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  padding: 1rem;
  margin: 0 1rem;
  border-bottom: 1px solid var(--color-border);
}
.item:last-child {
  border-bottom: none;
}
.main {
  text-align: center;
  margin: 0 auto;
}
.description {
  color: rgba(0, 0, 0, 0.5);
}
.temps {
  display: flex;
  gap: 1rem;
}
</style>
