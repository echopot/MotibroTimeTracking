<template>
  <div class="modal d-block" role="dialog">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header row g-3 d-flex justify-content-between">
          <div :class="deletable ? 'col-10' : 'col-12'">
            <input
              type="text"
              class="form-control form-control-lg"
              id="taskTitle"
              placeholder="Task Title"
              v-model="event.title"
            />
          </div>
          <div class="col-2 d-flex justify-content-end" v-if="deletable">
            <button type="button" class="btn btn-outline-danger" @click="deleteEvent">🗑️</button>
          </div>
        </div>
        <div class="modal-body">
          <form class="row g-3">
            <div class="col-12">
              <label for="contentTextarea">Task Content</label>
              <textarea
                class="form-control"
                id="contentTextarea"
                placeholder="Task Content"
                rows="3"
                v-model="event.content"
              ></textarea>
            </div>
            <div class="col-md-6">
              <label for="startDatetime" class="form-label">Start datetime</label>
              <input
                type="datetime-local"
                class="form-control"
                id="startDatetime"
                v-model="eventStart"
              />
            </div>
            <div class="col-md-6">
              <label for="endDatetime" class="form-label">End datetime</label>
              <input
                type="datetime-local"
                class="form-control"
                id="endDatetime"
                v-model="eventEnd"
              />
            </div>
            <div class="col-12">
              <label for="projectSelect">Choose Project</label>
              <select class="form-select" id="projectSelect" v-model="event.project">
                <option v-for="project in projects" :key="project.id" :value="project">
                  {{ project.name }}
                </option>
              </select>
            </div>
            <div class="col-12">
              <label for="clientSelect">Choose Client</label>
              <select class="form-select" id="clientSelect" v-model="event.client">
                <option v-for="client in clients" :key="client.id" :value="client">
                  {{ client.name }}
                </option>
              </select>
            </div>
          </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-primary" @click="saveEvent">Save</button>
          <button type="button" class="btn btn-secondary" @click="close">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'EventForm',
  emits: ['delete', 'cancel', 'save'],
  props: {
    selectedEvent: Object,
    projects: Array,
    clients: Array,
    deletable: Boolean,
  },
  setup(props, context) {
    const event = ref({ ...props.selectedEvent })

    // Convert event start and end to local time.
    const eventStart = ref(
      new Date(event.value.start)
        .toLocaleString('sv-SE', {
          timeZone: 'Europe/Budapest',
          hour12: false,
        })
        .replace(' ', 'T'),
    )
    const eventEnd = ref(
      new Date(event.value.end)
        .toLocaleString('sv-SE', {
          timeZone: 'Europe/Budapest',
          hour12: false,
        })
        .replace(' ', 'T'),
    )

    function deleteEvent() {
      context.emit('delete', event.value)
    }

    function saveEvent() {
      context.emit('save', event.value, eventStart.value, eventEnd.value)
    }

    function close() {
      context.emit('cancel')
    }

    return { event, eventStart, eventEnd, deleteEvent, saveEvent, close }
  },
}
</script>
