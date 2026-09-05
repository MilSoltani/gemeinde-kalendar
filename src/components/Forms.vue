<script setup lang="ts">
import { useCalendarStore } from '@/stores/useCalendarStore'

const calendarStore = useCalendarStore()

function printCalendar() {
  window.print()
}
</script>

<template>
  <section class="crud">
    <div class="crud-column">
      <h4>Anfangsmonat auswählen:</h4>

      <div class="form">
        <input
          v-model="calendarStore.selectedMonth"
          type="month"
        >

        <p class="selected-months">
          {{
            new Date(calendarStore.startYear, calendarStore.startMonth)
              .toLocaleString('de-DE', {
                month: 'long',
                year: 'numeric',
              })
          }}
          —
          {{
            new Date(calendarStore.startYear, calendarStore.startMonth + 1)
              .toLocaleString('de-DE', {
                month: 'long',
                year: 'numeric',
              })
          }}
        </p>

        <button @click="printCalendar">
          Drucken / PDF speichern
        </button>
      </div>
    </div>
  </section>
</template>

<style scoped>
.crud {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
  margin-bottom: 2rem;
}

.crud-column {
  padding: 1rem;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-surface);
}

.form {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

button:disabled {
  opacity: 0.5;
  pointer-events: none;
}

input,
textarea,
button {
  font-family: inherit;
}

textarea {
  padding: 0.5rem;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  resize: vertical;
}

.checkbox-group {
  display: flex;
  flex-direction: row;
  gap: 4px;
  font-family: inherit;
}

.time-selector {
  height: 20px;
}

@media print {
  .crud {
    display: none !important;
  }
}
</style>
