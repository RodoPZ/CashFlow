<script setup lang="ts">
import SplashScreen from "@/components/SplashScreen.vue";
import { defineAsyncComponent, type Component } from "vue";
const HomeView: Component = defineAsyncComponent(
  () =>
    new Promise((resolve) => {
      setTimeout(() => {
        resolve(import("./components/HomeView.vue"));
      }, 2500);
    })
);
</script>

<template>
  <Suspense>
    <template #default> <HomeView /></template>
    <template #fallback> <SplashScreen /></template>
  </Suspense>
</template>

<style>
html,
body,
.app {
  min-height: 100vh;
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}
* {
  --brand-green: #04b500;
  --brand-blue: #0689b0;
}
</style>
