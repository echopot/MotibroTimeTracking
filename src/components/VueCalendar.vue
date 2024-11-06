<template>
  <vue-cal
    small
    :time-from="7 * 60"
    :time-to="20 * 60"
    :disable-views="['years', 'year']"
    hide-title-bar
    :editable-events="{ title: true, drag: true, resize: true, create: true }"
    :events="events"
    :on-event-create="onEventCreate"
    :on-event-click="onEventClick"
    @event-drag-create="showEventCreationDialog = true"
  >
  </vue-cal>
</template>

<script setup>
import { ref } from 'vue'
import VueCal from 'vue-cal'

const { events } = defineProps(['events'])
const selectedEvent = ref(null)
const showEventCreationDialog = ref(false)

function onEventCreate(event) {
  console.log('Event created:', event)
  selectedEvent.value = event
  showEventCreationDialog.value = true

  return event
}

function onEventClick(event, e) {
  console.log('Event clicked:', event)
  this.selectedEvent = event

  // Prevent navigating to narrower view (default vue-cal behavior).
  e.stopPropagation()
}
</script>
