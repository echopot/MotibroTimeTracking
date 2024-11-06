<template>
  <header class="d-flex justify-content-center align-items-center">
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="25" height="25" />
    <h1>MotiBro test</h1>
  </header>

  <main>
    <vue-calendar
      :events="events"
      :projects="projects"
      :clients="clients"
      @delete-event="deleteEvent"
      @update-event="updateEvent"
      @create-event="createEvent"
    />
    <div
      v-if="showAlert.visible"
      :class="
        'alert ' +
        showAlert.className +
        ' fade show w-auto d-flex align-items-center fixed-bottom mx-auto'
      "
      role="alert"
      style="max-width: fit-content"
    >
      {{ showAlert.message }}
    </div>
  </main>
</template>

<script>
import VueCalendar from './components/VueCalendar.vue'
import { ref } from 'vue'

export default {
  components: { VueCalendar },
  name: 'App',
  setup() {
    const showAlert = ref({
      message: '',
      className: 'alert-success',
      visible: false,
    })

    const projects = [
      { id: 0, bg: '#eae2b7', color: '#000000', name: 'No Project', class: 'no-project' },
      { id: 1, bg: '#006d77', color: '#ffffff', name: 'Project 1', class: 'project-1' },
      { id: 2, bg: '#3a5a40', color: '#000000', name: 'Project 2', class: 'project-2' },
      { id: 3, bg: '#7f5539', color: '#ffffff', name: 'Project 3', class: 'project-3' },
    ]
    const clients = [
      { id: 0, bg: '#eae2b7', color: '#000000', name: 'No Client', class: 'no-client' },
      { id: 1, bg: '#83c5be', color: '#000000', name: 'Client 1', class: 'client-1' },
      { id: 2, bg: '#588157', color: '#000000', name: 'Client 2', class: 'client-2' },
      { id: 3, bg: '#ddb892', color: '#000000', name: 'Client 3', class: 'client-3' },
    ]

    // Dynamically add CSS rules for projects and clients.
    const styleElement = document.createElement('style')
    document.head.appendChild(styleElement)

    addStyles(projects)
    addStyles(clients)

    function addStyles(items) {
      items.forEach((item) => {
        const cssRule = `.${item.class} { background-color: ${item.bg}; color: ${item.color}; }`
        styleElement.sheet.insertRule(cssRule, styleElement.sheet.cssRules.length)
      })
    }

    // Create some test events.
    const events = ref([
      {
        id: 0,
        start: '2024-11-06 11:00',
        end: '2024-11-06 12:00',
        title: 'Test event for a Client',
        content: 'This is a test event for a Client.',
        project: projects[0],
        client: clients[1],
        class: clients[1].class,
      },
      {
        id: 1,
        start: '2024-11-06 14:00',
        end: '2024-11-06 18:00',
        title: 'Test event for a Project',
        content: 'This is a test event for a Project.',
        project: projects[1],
        client: clients[0],
        class: projects[1].class,
      },
    ])

    function deleteEvent(event) {
      const index = events.value.findIndex((e) => e.id === event.id)
      events.value.splice(index, 1)
      showAlertMessage('Event deleted successfully!', 'alert-success')
    }

    function updateEvent(event) {
      event.class = event.project.class !== 'no-project' ? event.project.class : event.client.class
      const index = events.value.findIndex((e) => e.id === event.id)
      events.value[index] = event
      showAlertMessage('Event updated successfully!', 'alert-success')
    }

    function createEvent(event) {
      event.id = events.value.length
      event.class = event.project.class !== 'no-project' ? event.project.class : event.client.class
      events.value.push(event)
      showAlertMessage('Event created successfully!', 'alert-success')
    }

    function showAlertMessage(message, className) {
      showAlert.value.message = message
      showAlert.value.className = className
      showAlert.value.visible = true
      hideAlertAfterDelay()
    }

    function hideAlertAfterDelay() {
      setTimeout(() => {
        showAlert.value.visible = false
      }, 3000)
    }

    return { showAlert, events, projects, clients, deleteEvent, updateEvent, createEvent }
  },
}
</script>
