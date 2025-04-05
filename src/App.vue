<script setup lang="ts">
import MainNav from './components/MainNav.vue'
</script>

<template>
  <header class="header">
    <h1 class="heading">Simple Weather</h1>
  </header>

  <main>
    <MainNav />
  </main>
</template>

<style scoped>
.header {
  padding: 0.25rem 1rem;
  background-color: var(--vt-c-indigo);
  color: var(--vt-c-text-dark-1);
}
.heading {
  font-size: 1.5rem;
  font-weight: 500;
}
</style>
