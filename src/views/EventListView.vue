<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
import EventMeta from '@/components/EventMeta.vue'
import type { Event } from '@/types'
import { ref, onMounted, computed, watchEffect } from 'vue'
import EventService from '@/services/EventService'
import { useRouter } from 'vue-router'

const props = defineProps({
  page: {
    type: Number,
    required: true
  },
  pageSize: {
    type: Number,
    required: true
  }
})

const router = useRouter()
const selectedPageSize = ref(props.pageSize)

const page = computed(() => props.page)
const events = ref<Event[] | null>(null)
const totalEvents = ref(0)
const hasNextPage = computed(() => {
  const totalPages = Math.ceil(totalEvents.value / 3)
  return page.value < totalPages
})

onMounted(() => {
  watchEffect(() => {
    EventService.getEvents(3, page.value)
      .then((response) => {
        console.log(response.data)
        events.value = response.data
        totalEvents.value = response.headers['x-total-count']
      })
      .catch((error) => {
        console.error('There was an error!', error)
      })
  })
})

const updatePageSize = () => {
  router.push({
    name: 'event-list-view',
    query: { page: '1', pageSize: selectedPageSize.value.toString() }
  })
}
</script>

<template>
  <h1>Events For Good</h1>
  <div class="page-size-selector">
    <label for="pageSize">Items per page: </label>
    <select id="pageSize" v-model="selectedPageSize" @change="updatePageSize">
      <option value="2">2</option>
      <option value="3">3</option>
      <option value="5">5</option>
    </select>
  </div>
  <div class="flex flex-col items-center">
      <div v-for="event in events" :key="event.id" class="event-container">
        <EventCard :event="event" />
        <EventMeta :event="event" />
      </div>
      <div class="pagination">
        <RouterLink
          id="page-prev"
          :to="{ name: 'event-list-view', query: { page: page - 1 } }"
          rel="prev"
          v-if="page != 1"
          >&#60; Prev Page</RouterLink
        >

        <RouterLink
          id="page-next"
          :to="{ name: 'event-list-view', query: { page: page + 1 } }"
          rel="next"
          v-if="hasNextPage"
          >Next Page &#62;</RouterLink
        >
      </div>
    </div>
</template>

<style scoped>
.event-container {
  margin-bottom: 20px;
}
.pagination {
  display: flex;
  width: 290px;
}
.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}

.page-size-selector {
  margin-bottom: 20px;
}

.page-size-selector select {
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-left: 10px;
}

#page-prev {
  text-align: left;
}

#page-next {
  text-align: right;
}
</style>