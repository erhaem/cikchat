<template>
  <div :class="['flex mb-4', isOwn ? 'justify-end' : 'justify-start']">
    <div :class="[
      'max-w-md md:max-w-lg px-4 py-2 rounded-lg shadow-sm',
      isOwn 
        ? 'bg-green-100 rounded-br-none' 
        : 'bg-white rounded-bl-none'
    ]">
      <div v-if="!isOwn" class="text-xs font-semibold text-purple-600 mb-1">
        {{ senderName }}
      </div>

      <div class="text-gray-800 text-base leading-relaxed">
        {{ message.message }}
      </div>

      <div class="text-xs text-gray-500 mt-1 text-right">
        ID: {{ message.id }}
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  message: {
    type: Object,
    required: true
  },
  senderName: {
    type: String,
    required: true
  },
  isOwn: {
    type: Boolean,
    required: true
  }
})
</script>
