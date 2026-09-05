<script setup lang="ts">
import { useCalendarStore } from '@/stores/useCalendarStore'

const calendarStore = useCalendarStore()

function printCalendar() {
  window.print()
}
</script>

<template>
  <section class="controls">
    <div class="controls-left">
      <button @click="printCalendar">
        Drucken / PDF speichern
      </button>
    </div>
    <div class="controls-right">
      <div class="form">
        <input
          v-model="calendarStore.selectedMonth"
          type="month"
        >
        <span class="selected-months">
          {{
            new Date(calendarStore.startYear, calendarStore.startMonth)
              .toLocaleString('de-DE', { month: 'short', year: 'numeric' })
          }}
          -
          {{
            new Date(calendarStore.startYear, calendarStore.startMonth + 1)
              .toLocaleString('de-DE', { month: 'short', year: 'numeric' })
          }}
        </span>
      </div>
    </div>
  </section>
</template>

<style scoped>
.controls {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 2rem;
  padding: 0.5rem 1rem;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-surface);
}

.controls-left,
.controls-right {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.controls-right {
  align-items: center;
}

.form {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.selected-months {
  font-size: 0.9em;
}

button:disabled {
  opacity: 0.5;
  pointer-events: none;
}

input,
button {
  font-family: inherit;
}

@media print {
  .controls {
    display: none !important;
  }
}
</style>
