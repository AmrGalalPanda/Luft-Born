<script setup>
import { ref, computed } from 'vue';
import AppHome from './components/AppHome.vue';
import AppAbout from './components/AppAbout.vue';
import AppProducts from './components/AppProducts.vue';
import AppHeader from './components/AppHeader.vue';
import AppFooter from './components/AppFooter.vue';

const routes = {
  '/': AppProducts,
  '/home': AppHome,
  '/about': AppAbout,
  '/AppProducts': AppProducts
};

const currentPath = ref(window.location.hash);

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash;
});

const currentView = computed(() => routes[currentPath.value.slice(1) || '/']);
</script>

<template>
  <div class="page-container">
    <AppHeader />
    <main class="content">
      <component :is="currentView" />
    </main>
    <AppFooter />
  </div>
</template>

<style>
body {
  background-color: #0E1323;
}

.page-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.content {
  flex: 1;
}
</style>
