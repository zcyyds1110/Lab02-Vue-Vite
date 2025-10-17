<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
import type { Event } from '@/types'
import { ref, onMounted } from 'vue'
import axios from 'axios'

const events = ref<Event[] | null>(null)

onMounted(() => {
  axios
    .get('https://my-json-server.typicode.com/zcyyds1110/Lab02-Vue-Vite/events')
    .then((response) => {
      console.log(response.data)
      events.value = response.data
    })
    .catch((error) => {
      console.error('There was an error!', error)
    })
})
</script>

<template>
  <h1>Events For Good</h1>
  <div v-for="event in events" :key="event.id" class="event-container">
    <EventCard :event="event" />
    <EventMeta :event="event" />
  </div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.event-container {
  margin-bottom: 20px;
}
</style>