<template>
  <div class="relative bg-white p-4 border-t border-gray-200 flex gap-3 items-center">
    <div class="flex-1 flex items-center bg-gray-100 rounded-full px-4 py-2">
      <input 
        type="text" 
        v-model="messageText" 
        @keyup.enter="sendMessage" 
        placeholder="Type a message..."
        class="flex-1 bg-transparent outline-none text-base"
      >
    </div>

    <button 
      @click="sendMessage" 
      :disabled="!messageText.trim()" 
      :class="[
        'w-10 h-10 rounded-full flex items-center justify-center transition',
        messageText.trim() 
          ? 'bg-purple-600 hover:bg-purple-700 text-white' 
          : 'bg-gray-300 text-gray-500 cursor-not-allowed'
      ]"
    >
      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8" />
      </svg>
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['send-message'])

const messageText = ref('')

const sendMessage = () => {
  if (!messageText.value.trim()) return
  
  emit('send-message', messageText.value)
  messageText.value = ''
}
</script>
