<template>
  <vue-cal
    small
    :time-from="7 * 60"
    :time-to="20 * 60"
    :disable-views="['years', 'year']"
    hide-title-bar
    :editable-events="{ title: true, drag: true, resize: true, create: true }"
    :events="savedEvents"
    :on-event-click="onEventClick"
    @event-drag-create="onEventCreate"
  >
  </vue-cal>
  <!-- Form for editing Events -->
  <event-form
    v-if="selectedEvent != null"
    :selectedEvent="selectedEvent"
    :projects="projects"
    :clients="clients"
    :deletable="true"
    @delete="deleteEvent"
    @save="saveEvent"
    @cancel="closeEventEditDialog"
  ></event-form>
  <!-- Form for new Events -->
  <event-form
    v-if="newEvent != null"
    :selectedEvent="newEvent"
    :projects="projects"
    :clients="clients"
    :deletable="false"
    @delete="deleteNewEvent"
    @save="createEvent"
    @cancel="closeEventCreateDialog"
  ></event-form>
</template>

<script>
import { ref } from 'vue'
import EventForm from './EventFrom.vue'
import VueCal from 'vue-cal'

export default {
  name: 'VueCalendar',
  components: { VueCal, EventForm },
  emits: ['delete-event', 'update-event', 'create-event'],
  props: ['events', 'projects', 'clients'],
  setup(props, context) {
    const savedEvents = ref([...props.events])
    const newEvent = ref(null)
    const selectedEvent = ref(null)

    function onEventCreate(event) {
      event.project = props.projects[0]
      event.client = props.clients[0]
      newEvent.value = event

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

    function deleteNewEvent() {
      newEvent.value = null
    }

    function createEvent(event, eventStart, eventEnd) {
      event.start = new Date(eventStart).toString()
      event.end = new Date(eventEnd).toString()
      context.emit('create-event', event)
      closeEventCreateDialog()
    }

    function saveEvent(event, eventStart, eventEnd) {
      event.start = new Date(eventStart).toString()
      event.end = new Date(eventEnd).toString()
      context.emit('update-event', event)
      closeEventEditDialog()
    }

    function closeEventEditDialog() {
      selectedEvent.value = null
    }

    function closeEventCreateDialog() {
      savedEvents.value = [...props.events]
      newEvent.value = null
    }

    return {
      savedEvents,
      newEvent,
      selectedEvent,
      onEventCreate,
      onEventClick,
      deleteEvent,
      deleteNewEvent,
      createEvent,
      saveEvent,
      closeEventEditDialog,
      closeEventCreateDialog,
    }
  },
}
</script>
