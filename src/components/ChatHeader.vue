<template>
  <div class="bg-[#01416c] text-white p-4 flex items-center gap-3 shadow-lg">
    <img :src="room.image_url" :alt="room.name" class="w-12 h-12 rounded-full border-2 border-white object-cover">
    <div class="flex-1">
      <h2 class="text-lg font-semibold">{{ room.name }}</h2>
      <p class="text-sm opacity-90">Room ID: {{ room.id }}</p>
    </div>
  </div>
</template>

<script setup>
defineProps({
  room: {
    type: Object,
    required: true
  }
})
</script>
