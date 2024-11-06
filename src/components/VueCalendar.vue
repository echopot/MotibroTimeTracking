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

<script>
import { ref } from 'vue'
import VueCal from 'vue-cal'

export default {
  name: 'VueCalendar',
  components: { VueCal },
  props: ['events', 'projects', 'clients'],
  setup() {
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
      selectedEvent.value = event

      // Prevent navigating to narrower view (default vue-cal behavior).
      e.stopPropagation()
    }

    return { selectedEvent, showEventCreationDialog, onEventCreate, onEventClick }
  },
}
</script>
