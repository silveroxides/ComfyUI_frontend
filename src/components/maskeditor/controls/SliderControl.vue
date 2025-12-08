<template>
  <div class="flex flex-col gap-1.5">
    <div class="flex items-center justify-between">
      <span class="text-left text-xs font-sans text-[var(--descrip-text)]">{{
        label
      }}</span>
      <div class="flex items-center gap-1">
        <button
          class="slider-step-btn"
          :aria-label="t('g.decrement', { label })"
          :disabled="modelValue <= min"
          @click="decrement"
        >
          <i class="pi pi-chevron-left text-[10px]" />
        </button>
        <span
          class="text-xs font-sans text-[var(--descrip-text)] min-w-8 text-center tabular-nums"
        >
          {{ displayValue }}
        </span>
        <button
          class="slider-step-btn"
          :aria-label="t('g.increment', { label })"
          :disabled="modelValue >= max"
          @click="increment"
        >
          <i class="pi pi-chevron-right text-[10px]" />
        </button>
      </div>
    </div>
    <input
      type="range"
      class="maskEditor_sidePanelBrushRange"
      :min="min"
      :max="max"
      :step="step"
      :value="modelValue"
      @input="onInput"
    />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

import { t } from '@/i18n'

const {
  min,
  max,
  step = 1,
  modelValue
} = defineProps<{
  label: string
  min: number
  max: number
  step?: number
  modelValue: number
}>()

const emit = defineEmits<{
  'update:modelValue': [value: number]
}>()

const displayValue = computed(() => {
  // Show up to 2 decimal places, but remove trailing zeros
  if (step < 1) {
    return Number(modelValue.toFixed(2))
  }
  return Math.round(modelValue)
})

const onInput = (event: Event) => {
  const value = Number((event.target as HTMLInputElement).value)
  emit('update:modelValue', value)
}

const increment = () => {
  const newValue = Math.min(max, modelValue + step)
  emit('update:modelValue', newValue)
}

const decrement = () => {
  const newValue = Math.max(min, modelValue - step)
  emit('update:modelValue', newValue)
}
</script>

<style scoped>
.slider-step-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 18px;
  height: 18px;
  border-radius: 4px;
  border: 1px solid var(--p-form-field-border-color);
  background: var(--comfy-menu-bg);
  color: var(--descrip-text);
  cursor: pointer;
  transition: background-color 0.1s;
}

.slider-step-btn:hover:not(:disabled) {
  background: var(--p-button-text-primary-hover-background);
}

.slider-step-btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
</style>
