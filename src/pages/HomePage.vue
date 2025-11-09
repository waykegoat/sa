<template>
  <div class="home-page">
    <div class="page-container">
      <h1 class="page-title">🖧 Калькулятор подсетей</h1>
      <p class="page-description">
        Введите IP адрес и выберите сетевую маску для расчета параметров подсети
      </p>
      <SubnetCalculator />
    </div>
  </div>
</template>

<script setup lang="ts">
import SubnetCalculator from '../components/SubnetCalculator.vue'
</script>

<style scoped>
.home-page {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 40px 20px;
}

.page-container {
  max-width: 800px;
  margin: 0 auto;
}

.page-title {
  text-align: center;
  color: var(--color-white);
  font-size: 32px;
  margin-bottom: 16px;
  font-weight: 700;
}

.page-description {
  text-align: center;
  color: var(--color-white);
  font-size: 18px;
  margin-bottom: 40px;
  opacity: 0.9;
}
</style>