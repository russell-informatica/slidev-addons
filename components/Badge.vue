<script setup lang="ts">
withDefaults(defineProps<{
  accent?: boolean
  round?: boolean
}>(), {
  accent: false,
  round: false,
})
</script>

<template>
  <span class="badge" :class="{ 'badge-accent': accent, 'badge-round': round }">
    <slot />
  </span>
</template>
