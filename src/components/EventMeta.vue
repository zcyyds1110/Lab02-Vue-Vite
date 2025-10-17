<script setup lang="ts">
import type { Event } from '@/types'

defineProps<{
  event: Event
}>()
</script>

<template>
  <div class="event-meta">
    <div class="meta-item">{{ event.category }}</div>
    <div class="meta-item">{{ event.organizer }}</div>
  </div>
</template>

<style scoped>
.event-meta {
  width: 250px;
  text-align: right;
}

.meta-item {
  font-size: 14px;
  margin-bottom: 5px;
}
</style>