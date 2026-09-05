<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'
import { useFormStore } from '@/stores/useFormStore'
import TimePicker from './TimePicker.vue'

const formStore = useFormStore()

function closeModal() {
  formStore.showModal = false
  formStore.resetForm()
}

function handleKeydown(event: KeyboardEvent) {
  if (event.key === 'Escape') {
    closeModal()
  }
}

onMounted(() => window.addEventListener('keydown', handleKeydown))
onUnmounted(() => window.removeEventListener('keydown', handleKeydown))

function handleSubmit() {
  if (formStore.canSubmit) {
    if (formStore.isEditing) {
      formStore.updateEvent()
    }
    else {
      formStore.addEvent()
    }
    closeModal()
  }
}

function handleDelete() {
  if (formStore.isEditing) {
    formStore.deleteEvent()
    closeModal()
  }
}
</script>

<template>
  <div v-if="formStore.showModal" class="modal-overlay" @click.self="closeModal">
    <div class="modal">
      <h3>{{ formStore.isEditing ? 'Termin bearbeiten' : 'Termine hinzufügen' }}</h3>

      <div class="form">
        <input
          v-model="formStore.form.title"
          type="text"
          placeholder="Titel"
        >

        <div>
          <input
            v-model="formStore.form.date"
            type="date"
          >
        </div>

        <div class="checkbox-group">
          <input
            id="wholeDayModal"
            v-model="formStore.form.wholeDay"
            type="checkbox"
          >

          <label for="wholeDayModal">Ganzer Tag</label>
        </div>

        <div class="time-selector">
          <TimePicker
            v-if="!formStore.form.wholeDay"
            v-model="formStore.form.time"
          />
        </div>

        <textarea
          v-model="formStore.form.notes"
          placeholder="Notizen (optional)"
          rows="3"
        />

        <div class="buttons">
          <button
            :disabled="!formStore.canSubmit"
            @click="handleSubmit"
          >
            {{ formStore.isEditing ? 'Aktualisieren' : 'Hinzufügen' }}
          </button>
          <button v-if="formStore.isEditing" @click="handleDelete">
            Löschen
          </button>
          <button @click="closeModal">
            Abbrechen
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.modal {
  background: var(--color-surface);
  padding: 1.5rem;
  border-radius: var(--radius-sm);
  max-width: 400px;
  width: 90%;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
}

.modal h3 {
  margin: 0 0 1rem;
  text-align: center;
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
  justify-content: flex-end;
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
</style>
