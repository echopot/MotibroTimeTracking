<template>
  <vue-cal
    small
    :time-from="7 * 60"
    :time-to="20 * 60"
    :disable-views="['years', 'year']"
    hide-title-bar
    :editable-events="{ title: true, drag: true, resize: true, create: true }"
    :events="events"
    :on-event-click="onEventClick"
    @event-drag-create="showEventCreationDialog = true"
  >
  </vue-cal>
  <event-form
    v-if="selectedEvent != null"
    :selectedEvent="selectedEvent"
    :projects="projects"
    :clients="clients"
    @delete="deleteEvent"
    @save="saveEvent"
    @cancel="closeEventEditDialog"
  ></event-form>
</template>

<script>
import { ref } from 'vue'
import EventForm from './EventFrom.vue'
import VueCal from 'vue-cal'

export default {
  name: 'VueCalendar',
  components: { VueCal, EventForm },
  emits: ['delete-event', 'update-event'],
  props: ['events', 'projects', 'clients'],
  setup(props, context) {
    const selectedEvent = ref(null)
    const showEventCreationDialog = ref(false)

    function onEventCreate(event) {
      console.log('Event created:')
      selectedEvent.value = event
      showEventCreationDialog.value = true

      return event
    }

    function onEventClick(event, e) {
      console.log('Event clicked:')
      selectedEvent.value = event

      // Prevent navigating to narrower view (default vue-cal behavior).
      e.stopPropagation()
    }

    function deleteEvent(event) {
      context.emit('delete-event', event)
      closeEventEditDialog()
    }

    function saveEvent(event, eventStart, eventEnd) {
      event.start = new Date(eventStart).toString()
      event.end = new Date(eventEnd).toString()
      context.emit('update-event', event)
      closeEventEditDialog()
    }

    function closeEventEditDialog() {
      selectedEvent.value = null
      showEventCreationDialog.value = false
    }

    return {
      selectedEvent,
      showEventCreationDialog,
      onEventCreate,
      onEventClick,
      deleteEvent,
      saveEvent,
      closeEventEditDialog,
    }
  },
}
</script>
