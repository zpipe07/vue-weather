<script setup lang="ts">
import { ref } from 'vue'
import type { WeatherData, WeatherListItem } from '../types'

const props = defineProps<{
  data: WeatherData
}>()

const nextFiveDays = ref(
  props.data.list.reduce(
    (
      acc: Record<
        string,
        { high: number; low: number; icon: string; description: string; day: string }
      >,
      item: WeatherListItem,
    ) => {
      const key = item.dt_txt.split(' ')[0]
      if (!acc[key]) {
        acc[key] = {
          high: item.main.temp_max,
          low: item.main.temp_min,
          icon: item.weather[0].icon,
          description: item.weather[0].description,
          day: new Date(item.dt * 1000).toDateString(),
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
  <div v-if="data" class="card">
    <h2 class="heading">Next 5 days</h2>
    <ul v-if="data" class="list">
      <li class="item" v-for="(item, key) in nextFiveDays" :key="key">
        <img
          :src="'http://openweathermap.org/img/wn/' + item.icon + '@2x.png'"
          alt="Weather icon"
          width="100"
          height="100"
        />
        <div class="main">
          <h3>{{ item.day }}</h3>
          <h4>{{ item.description }}</h4>
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
.temps {
  display: flex;
  gap: 1rem;
}
</style>
