<template>
  <HomePage />
</template>

<script setup lang="ts">
import HomePage from './pages/HomePage.vue'
</script>

<style>
:root {
  --color-primary: #006cbb;
  --color-success: #01bd78;
  --color-error: #d40019;
  --color-white: #fff;
  --color-gray: #ccc;
  --color-black: #000;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  line-height: 1.6;
  color: var(--color-black);
}
</style>