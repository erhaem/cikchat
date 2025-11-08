<template>
  <div class="relative bg-white p-4 border-t border-gray-200 flex gap-3 items-center">
    <button 
      type="button"
      @click="handleAttach"
      class="p-2 hover:bg-gray-100 rounded-full transition"
    >
      <svg class="w-6 h-6 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
          d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13" />
      </svg>
    </button>

    <input
      ref="fileInput"
      type="file"
      class="hidden"
      @change="handleFileChange"
    >

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
          ? 'bg-[#01416c] hover:bg-[#026aa7] text-white' 
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

const emit = defineEmits(['send-message', 'attach-file'])

const messageText = ref('')
const fileInput = ref(null)

const sendMessage = () => {
  if (!messageText.value.trim()) return
  
  emit('send-message', messageText.value)
  messageText.value = ''
}

const handleAttach = () => {
  if (fileInput.value) {
    fileInput.value.click()
  }
}

const handleFileChange = (event) => {
  const [file] = event.target.files || []
  if (!file) return

  emit('attach-file', file)
  event.target.value = ''
}
</script>
