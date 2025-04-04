<script setup lang="ts">
import { ref } from 'vue'
import WeatherInfo from '../components/WeatherInfo.vue'

const tabs = ['denver', 'rio de janeiro', 'beijing', 'los angeles'] as const

const emit = defineEmits(['update:selectedTab'])

const selectedTab = ref('denver')

const selectTab = (tab: string) => {
  selectedTab.value = tab
  emit('update:selectedTab', tab)
}
</script>

<template>
  <div class="tabs">
    <!-- TODO is this the most semantic approach? -->
    <a
      href="#"
      :class="['tab', { active: selectedTab === tab }]"
      v-for="(tab, index) in tabs"
      :key="index"
      @click="selectTab(tab)"
    >
      {{ tab }}
    </a>
  </div>
  <div class="tab-content">
    <WeatherInfo :city="selectedTab" />
  </div>
</template>

<style scoped>
.tabs {
  display: flex;
  gap: 1rem;
  margin-bottom: 1rem;
  justify-content: center;
}
.tab {
  padding: 0.5rem 1rem;
  background-color: #f0f0f0;
  border-radius: 4px;
  text-decoration: none;
  color: #333;
}
.tab:hover {
  background-color: #e0e0e0;
}
.tab:active {
  background-color: #d0d0d0;
}
.tab.selected {
  background-color: #ccc;
  font-weight: bold;
}
.tab.selected:hover {
  background-color: #bbb;
}
.tab.selected:active {
  background-color: #aaa;
}
.tab.active {
  background-color: #ccc;
}
.tab.active:hover {
  background-color: #bbb;
}
.tab.active:active {
  background-color: #aaa;
}
</style>
