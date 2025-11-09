<template>
  <div class="subnet-calculator">
    <h2 class="calculator-title">Калькулятор подсетей</h2>
    
    <div class="calculator-form">
      <div class="form-group">
        <label class="form-label" for="ip-address">IP адрес:</label>
        <input
          id="ip-address"
          v-model="ipAddress"
          type="text"
          placeholder="192.168.1.150"
          class="form-input"
          :class="{ 'input-error': !isIpValid(ipAddress) && ipAddress !== '' }"
          @keyup.enter="calculate"
        >
        <div v-if="!isIpValid(ipAddress) && ipAddress !== ''" class="error-message">
          Неверный формат IP адреса
        </div>
      </div>

      <div class="form-group">
        <label class="form-label" for="netmask">Сетевая маска:</label>
        <select
          id="netmask"
          v-model="selectedMask"
          class="form-select"
        >
          <option v-for="mask in MASK_OPTIONS" :key="mask.value" :value="mask.value">
            {{ mask.label }}
          </option>
        </select>
      </div>

      <button
        class="calculate-button"
        :disabled="!isFormValid"
        @click="calculate"
      >
        Рассчитать
      </button>
    </div>

    <div v-if="showResults" class="results-section">
      <h3 class="results-title">Результаты расчета:</h3>
      
      <div class="result-item">
        <span class="result-label">IP адрес:</span>
        <span class="result-value">{{ ipAddress }}</span>
      </div>
      
      <div class="result-item">
        <span class="result-label">Сетевая маска:</span>
        <span class="result-value">{{ selectedMaskLabel }}</span>
      </div>
      
      <div class="result-item">
        <span class="result-label">Адрес сети:</span>
        <span class="result-value network-address">{{ networkAddress }}</span>
      </div>
      
      <div class="result-item">
        <span class="result-label">Количество адресов:</span>
        <span class="result-value addresses-count">{{ addressesCount }}</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { MASK_OPTIONS } from '../constants/masks'
import { isIpValid, getNetworkAddress, getAddressesCount } from '../utils/ipCalculator'

const ipAddress = ref('192.168.1.150')
const selectedMask = ref('255.255.255.0')
const showResults = ref(false)

const isFormValid = computed(() => {
  return isIpValid(ipAddress.value)
})

const selectedMaskLabel = computed(() => {
  const mask = MASK_OPTIONS.find(m => m.value === selectedMask.value)
  return mask ? mask.label : selectedMask.value
})

const networkAddress = computed(() => {
  return getNetworkAddress(ipAddress.value, selectedMask.value)
})

const addressesCount = computed(() => {
  return getAddressesCount(selectedMask.value)
})

function calculate() {
  if (isFormValid.value) {
    showResults.value = true
  }
}
</script>

<style scoped>
.subnet-calculator {
  max-width: 500px;
  margin: 0 auto;
  padding: 24px;
  background-color: var(--color-white);
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.calculator-title {
  margin-bottom: 24px;
  text-align: center;
  color: var(--color-primary);
  font-size: 24px;
  font-weight: 700;
}

.calculator-form {
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-bottom: 24px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.form-label {
  font-weight: 600;
  color: var(--color-black);
}

.form-input,
.form-select {
  padding: 12px;
  border: 2px solid var(--color-gray);
  border-radius: 4px;
  font-size: 16px;
  transition: border-color 0.3s ease;
}

.form-input:focus,
.form-select:focus {
  outline: none;
  border-color: var(--color-primary);
}

.form-input.input-error {
  border-color: var(--color-error);
}

.error-message {
  color: var(--color-error);
  font-size: 14px;
  margin-top: 4px;
}

.calculate-button {
  padding: 12px 24px;
  background-color: var(--color-primary);
  color: var(--color-white);
  border: none;
  border-radius: 4px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.calculate-button:hover:not(:disabled) {
  background-color: #0056a3;
}

.calculate-button:disabled {
  background-color: var(--color-gray);
  cursor: not-allowed;
}

.results-section {
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border-left: 4px solid var(--color-success);
}

.results-title {
  margin-bottom: 16px;
  color: var(--color-primary);
  font-size: 20px;
}

.result-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 0;
  border-bottom: 1px solid var(--color-gray);
}

.result-item:last-child {
  border-bottom: none;
}

.result-label {
  font-weight: 600;
  color: var(--color-black);
}

.result-value {
  color: var(--color-primary);
  font-family: monospace;
}

.network-address {
  color: var(--color-success);
  font-weight: 700;
}

.addresses-count {
  color: var(--color-error);
  font-weight: 700;
}
</style>