<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
import EventMeta from '@/components/EventMeta.vue'
import type { Event } from '@/types'
import { ref, onMounted } from 'vue'

const events = ref<Event[]>([])
const isLoading = ref<boolean>(true)
const error = ref<string | null>(null)

const fetchEvents = async () => {
  try {
    isLoading.value = true
    // 从my-json-server获取数据
    const response = await fetch('https://my-json-server.typicode.com/zcyyds1110/Lab02-Vue-Vite/events')
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    const data = await response.json()
    events.value = data
  } catch (err) {
    console.error('Failed to fetch events:', err)
    error.value = 'Failed to load events. Please try again later.'
    // 如果API请求失败，使用备用的硬编码数据
    events.value = [
      {
        id: 5928101,
        category: 'animal welfare',
        title: 'Cat Adoption Day',
        description: 'Find your new feline friend at this event.',
        location: 'Meow Town',
        date: 'January 28, 2022',
        time: '12:00',
        petsAllowed: true,
        organizer: 'Kat Laydee'
      },
      {
        id: 4582797,
        category: 'food',
        title: 'Community Gardening',
        description: 'Join us as we tend to the community edible plants.',
        location: 'Flora City',
        date: 'March 14, 2022',
        time: '10:00',
        petsAllowed: true,
        organizer: 'Fern Pollin'
      },
      {
        id: 8419988,
        category: 'sustainability',
        title: 'Beach Cleanup',
        description: 'Help pick up trash along the shore.',
        location: 'Playa Del Carmen',
        date: 'July 22, 2022',
        time: '11:00',
        petsAllowed: false,
        organizer: 'Carey Wales'
      }
    ]
  } finally {
    isLoading.value = false
  }
}

// 组件挂载时获取数据
onMounted(() => {
  fetchEvents()
})
</script>

<template>
  <h1>Events For Good</h1>
  
  <!-- 加载状态 -->
  <div v-if="isLoading" class="loading">
    Loading events...
  </div>
  
  <!-- 错误状态 -->
  <div v-else-if="error" class="error">
    {{ error }}
  </div>
  
  <!-- 事件列表 -->
  <div v-else class="events">
    <div v-if="events.length === 0" class="no-events">
      No events found.
    </div>
    <div v-for="event in events" :key="event.id" class="event-container">
      <EventCard :event="event" />
      <EventMeta :event="event" />
    </div>
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
.loading, .error, .no-events {
  padding: 20px;
  text-align: center;
  color: #666;
}
.error {
  color: #e74c3c;
}
</style>