<script setup lang="ts">
import Calendar from './components/Calendar.vue'
import EventModal from './components/EventModal.vue'
import Forms from './components/Forms.vue'
import Notes from './components/Notes.vue'
import { useCalendarStore } from './stores/useCalendarStore'

const calendarStore = useCalendarStore()
</script>

<template>
  <div class="document">
    <Forms />

    <section
      v-for="(month, index) in calendarStore.months"
      :key="index"
      class="page"
    >
      <Calendar :month="month" />
      <Notes :month="month" />
    </section>
  </div>

  <EventModal />
</template>

<style scoped>
</style>
