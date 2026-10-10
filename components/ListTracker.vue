<script setup lang="ts">
import { computed } from 'vue'
import { useNav } from '@slidev/client'

/**
 * One pointer following an index into `items` over the clicks of a slide.
 * `values` maps an absolute click number to the tracked index: at click `c`
 * the pointer shows the value of the greatest key `<= c`. `null` hides it.
 */
interface Tracker {
  name: string
  values: Record<number, number | null>
  color?: string
}

const props = withDefaults(
  defineProps<{
    /** Cells drawn in order, one per list element. */
    items: (string | number)[]
    /** Pointers to draw; either named object or an ordered array. */
    trackers: Tracker[] | Record<string, Record<number, number | null>>
    /** Highlight the cell currently pointed at. */
    highlight?: boolean
    /** Draw the small index ruler under the grid. */
    showIndexes?: boolean
  }>(),
  {
    highlight: false,
    showIndexes: true,
  },
)

const { clicks } = useNav()

const trackers = computed<Tracker[]>(() =>
  Array.isArray(props.trackers)
    ? props.trackers
    : Object.entries(props.trackers).map(([name, values]) => ({ name, values })),
)

function indexAt(values: Record<number, number | null>, click: number): number | null {
  let bestKey = -Infinity
  let index: number | null = null
  for (const [key, value] of Object.entries(values)) {
    const clickKey = Number(key)
    if (clickKey <= click && clickKey > bestKey) {
      bestKey = clickKey
      index = value
    }
  }
  return index
}

const active = computed(() =>
  trackers.value.map(tracker => ({
    ...tracker,
    index: indexAt(tracker.values, clicks.value),
  })),
)

const columns = computed(() => props.items.length)

// Center of cell `index` as a percentage of the grid width. Indices `-1` and
// `columns` land half a cell outside the grid, so loop-exit states stay visible.
function leftOf(index: number) {
  return ((index + 0.5) / columns.value) * 100
}
</script>

<template>
  <div class="list-tracker">
    <div class="list-tracker__lanes">
      <div v-for="tracker in active" :key="tracker.name" class="list-tracker__lane">
        <div
          v-if="tracker.index !== null"
          class="list-tracker__pointer"
          :style="{ left: `${leftOf(tracker.index)}%`, color: tracker.color }"
        >
          <span class="list-tracker__label">{{ tracker.name }}={{ tracker.index }}</span>
          <span class="list-tracker__arrow" />
        </div>
      </div>
    </div>

    <div
      class="list-tracker__grid"
      :style="{ gridTemplateColumns: `repeat(${columns}, minmax(0, 1fr))` }"
    >
      <div
        v-for="(item, index) in items"
        :key="index"
        class="list-tracker__cell"
        :class="{ 'list-tracker__cell--active': highlight && active.some(t => t.index === index) }"
      >
        {{ item }}
      </div>
    </div>

    <div
      v-if="showIndexes"
      class="list-tracker__grid list-tracker__ruler"
      :style="{ gridTemplateColumns: `repeat(${columns}, minmax(0, 1fr))` }"
    >
      <div v-for="(item, index) in items" :key="index" class="list-tracker__index">
        {{ index }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.list-tracker {
  position: relative;
  padding: 0 0.75rem;
}

.list-tracker__lanes {
  display: flex;
  flex-direction: column;
}

.list-tracker__lane {
  position: relative;
  height: 2.1rem;
}

.list-tracker__pointer {
  position: absolute;
  top: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: var(--slidev-theme-primary);
  transform: translateX(-50%);
  transition: left 260ms ease;
  white-space: nowrap;
}

.list-tracker__label {
  font-family: var(--slidev-code-font-family);
  font-size: 0.82em;
  line-height: 1.2;
}

.list-tracker__arrow {
  width: 0;
  height: 0;
  margin-top: 0.1rem;
  border-left: 0.4rem solid transparent;
  border-right: 0.4rem solid transparent;
  border-top: 0.5rem solid currentColor;
}

.list-tracker__grid {
  display: grid;
}

.list-tracker__cell {
  padding: 0.32rem 0.5rem;
  border: 1px solid var(--c-border);
  border-right: 0;
  background: var(--c-surface);
  color: var(--c-text-strong);
  font-family: var(--slidev-code-font-family);
  text-align: center;
}

.list-tracker__cell:last-child {
  border-right: 1px solid var(--c-border);
}

.list-tracker__cell--active {
  background: color-mix(in srgb, var(--slidev-theme-primary) 14%, transparent);
}

.list-tracker__ruler {
  margin-top: 0.15rem;
}

.list-tracker__index {
  color: var(--c-text-muted);
  font-family: var(--slidev-code-font-family);
  font-size: 0.7em;
  text-align: center;
}

@media (prefers-reduced-motion: reduce) {
  .list-tracker__pointer {
    transition: none;
  }
}
</style>
