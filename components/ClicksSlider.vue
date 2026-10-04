<script setup lang="ts">
import { computed } from 'vue'
import { useNav } from '@slidev/client'

const { clicks, clicksTotal, currentPage, go } = useNav()

const steps = computed(() => clicksTotal.value + 1)
// Reserve room for the widest label (`xx/xx`, digits derived from the total)
// so the rail does not shift when the number grows, e.g. 9 -> 10.
const labelMinWidth = computed(() => `${String(clicksTotal.value).length * 2 + 1}ch`)

function goToStep(step: number) {
  go(currentPage.value, step)
}
</script>

<template>
  <div v-if="clicksTotal" class="clicks-slider flex items-center gap-3 font-mono select-none">
    <span
      class="inline-block text-sm text-right tabular-nums whitespace-nowrap"
      :style="{ minWidth: labelMinWidth }"
    >
      <span class="text-primary">{{ clicks }}</span>
      <span class="opacity-25">/</span>
      <span class="text-xs opacity-50">{{ clicksTotal }}</span>
    </span>
    <div class="flex flex-1 gap-1">
      <button
        v-for="i in steps"
        :key="i"
        type="button"
        class="clicks-slider__step h-1.5 flex-1 cursor-pointer rounded-full p-0 transition-opacity hover:opacity-70"
        :class="i - 1 <= clicks ? 'bg-primary' : 'bg-[var(--c-border-strong)]'"
        :title="`Vai allo step ${i - 1}`"
        @mousedown.prevent
        @click="goToStep(i - 1)"
      />
    </div>
  </div>
</template>
