<template>
  <header class="d-flex justify-content-center align-items-center">
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="25" height="25" />
    <h1>MotiBro test</h1>
  </header>

  <main>
    <VueCalendar :events="events" :projects="projects" :clients="clients" />
  </main>
</template>

<script setup>
import VueCalendar from './components/VueCalendar.vue'
import { ref } from 'vue'

const projects = [
  { id: 0, bg: '#999999', color: '#ffffff', name: 'No Project', class: 'no-project' },
  { id: 1, bg: '#ff0000', color: '#ffffff', name: 'Project 1', class: 'project-1' },
  { id: 2, bg: '#00ff00', color: '#000000', name: 'Project 2', class: 'project-2' },
  { id: 3, bg: '#0000ff', color: '#ffffff', name: 'Project 3', class: 'project-3' },
]
const clients = [
  { id: 0, bg: '#666666', color: '#ffffff', name: 'No Client', class: 'no-client' },
  { id: 1, bg: '#bb0000', color: '#ffffff', name: 'Client 1', class: 'client-1' },
  { id: 2, bg: '#00bb00', color: '#000000', name: 'Client 2', class: 'client-2' },
  { id: 3, bg: '#0000bb', color: '#ffffff', name: 'Client 3', class: 'client-3' },
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
    start: '2024-11-06 11:00',
    end: '2024-11-06 12:00',
    title: 'Test event for a Client',
    content: 'This is a test event for a Client.',
    project: null,
    client: clients[1],
    class: clients[1].class,
  },
  {
    start: '2024-11-06 14:00',
    end: '2024-11-06 18:00',
    title: 'Test event for a Project',
    content: 'This is a test event for a Project.',
    project: projects[1],
    client: clients[1],
    class: projects[1].class,
  },
])
</script>
