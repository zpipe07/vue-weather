<script setup lang="ts">
import { ref } from 'vue'
import WeatherInfo from '../components/WeatherInfo.vue'

const tabs = ['rio de janeiro', 'beijing', 'los angeles'] as const

const emit = defineEmits(['update:selectedTab'])

const selectedTab = ref('rio de janeiro')

const selectTab = (tab: string) => {
  selectedTab.value = tab
  emit('update:selectedTab', tab)
}
</script>

<template>
  <!-- TODO is this the most semantic approach? -->
  <nav class="tabs">
    <a
      href="#"
      :class="['tab', { selected: selectedTab === tab }]"
      v-for="(tab, index) in tabs"
      :key="index"
      @click="selectTab(tab)"
    >
      {{ tab.toUpperCase() }}
    </a>
  </nav>

  <div class="content">
    <WeatherInfo :city="selectedTab" />
  </div>
</template>

<style scoped>
.tabs {
  display: flex;
  background-color: #f0f0f0;
  overflow-x: auto;
}
.tab {
  padding: 1rem;
  text-decoration: none;
  color: rgba(51, 51, 51, 0.7);
  transition: box-shadow 0.2s ease-in-out;
  font-weight: bold;
  white-space: nowrap;
}
.tab:hover {
  background-color: #e0e0e0;
}
.tab.selected {
  color: rgba(51, 51, 51, 1);
  box-shadow: 0px -3px 0px rgba(255, 0, 0, 0.8) inset;
}
.tab.selected:active {
  background-color: #aaa;
}
.tab.active {
  background-color: #ccc;
}
.tab.active:active {
  background-color: #aaa;
}
.content {
  padding: 1rem;
}
</style>
