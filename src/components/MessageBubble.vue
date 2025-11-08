<template>
  <div :class="['flex mb-4', isOwn ? 'justify-end' : 'justify-start']">
    <div :class="[
      'max-w-md md:max-w-lg px-4 py-2 rounded-lg shadow-sm',
      isOwn 
        ? 'bg-green-100 rounded-br-none' 
        : 'bg-white rounded-bl-none'
    ]">
  <div v-if="!isOwn" class="text-xs font-semibold text-[#01416c] mb-1">
        {{ senderName }}
      </div>

      <!-- Text message -->
      <div v-if="message.message" class="text-gray-800 text-base leading-relaxed mb-2">
        {{ message.message }}
      </div>

      <!-- Image attachment -->
      <div v-if="message.type === 'image' && message.attachments && message.attachments.length > 0" class="mt-2">
        <div v-for="(attachment, index) in message.attachments" :key="index">
          <img 
            :src="attachment.file_url" 
            :alt="attachment.file_name"
            class="rounded-lg max-w-full h-auto cursor-pointer hover:opacity-90 transition"
            @click="openMedia(attachment.file_url)"
          >
          <div class="text-xs text-gray-500 mt-1">
            {{ attachment.file_name }} ({{ formatFileSize(attachment.file_size) }})
          </div>
        </div>
      </div>

      <!-- Video attachment -->
      <div v-if="message.type === 'video' && message.attachments && message.attachments.length > 0" class="mt-2">
        <div v-for="(attachment, index) in message.attachments" :key="index">
          <video 
            controls 
            :poster="attachment.thumbnail_url"
            class="rounded-lg max-w-full h-auto"
          >
            <source :src="attachment.file_url" :type="attachment.file_type">
            Your browser does not support the video tag.
          </video>
          <div class="text-xs text-gray-500 mt-1">
            {{ attachment.file_name }} ({{ formatFileSize(attachment.file_size) }})
            <span v-if="attachment.duration"> • {{ formatDuration(attachment.duration) }}</span>
          </div>
        </div>
      </div>

      <!-- PDF attachment -->
      <div v-if="message.type === 'pdf' && message.attachments && message.attachments.length > 0" class="mt-2">
        <div v-for="(attachment, index) in message.attachments" :key="index" class="border border-gray-300 rounded-lg p-3 bg-gray-50">
          <div class="flex items-center gap-3">
            <div class="flex-shrink-0">
              <svg class="w-10 h-10 text-red-500" fill="currentColor" viewBox="0 0 20 20">
                <path d="M4 4a2 2 0 012-2h4.586A2 2 0 0112 2.586L15.414 6A2 2 0 0116 7.414V16a2 2 0 01-2 2H6a2 2 0 01-2-2V4z" />
                <path d="M8 10a.75.75 0 01.75-.75h2.5a.75.75 0 010 1.5h-2.5A.75.75 0 018 10zm0 3a.75.75 0 01.75-.75h2.5a.75.75 0 010 1.5h-2.5A.75.75 0 018 13z" />
              </svg>
            </div>
            <div class="flex-1 min-w-0">
              <p class="text-sm font-medium text-gray-900 truncate">{{ attachment.file_name }}</p>
              <p class="text-xs text-gray-500">{{ formatFileSize(attachment.file_size) }}</p>
            </div>
            <a 
              :href="attachment.file_url" 
              target="_blank"
              class="flex-shrink-0 px-3 py-1.5 bg-[#01416c] hover:bg-[#026aa7] text-white text-sm rounded-lg transition"
            >
              View
            </a>
          </div>
        </div>
      </div>

      <!-- Generic file attachment -->
      <div v-if="message.type === 'file' && message.attachments && message.attachments.length > 0" class="mt-2">
        <div v-for="(attachment, index) in message.attachments" :key="index" class="border border-gray-300 rounded-lg p-3 bg-gray-50">
          <div class="flex items-center gap-3">
            <div class="flex-shrink-0">
              <svg class="w-10 h-10 text-blue-500" fill="currentColor" viewBox="0 0 20 20">
                <path d="M4 4a2 2 0 012-2h6l4 4v10a2 2 0 01-2 2H6a2 2 0 01-2-2V4z" />
                <path d="M8 11h4M8 14h4" stroke="#1D4ED8" stroke-width="1.5" stroke-linecap="round" />
              </svg>
            </div>
            <div class="flex-1 min-w-0">
              <p class="text-sm font-medium text-gray-900 truncate">{{ attachment.file_name }}</p>
              <p class="text-xs text-gray-500">{{ formatFileSize(attachment.file_size) }}</p>
            </div>
            <a 
              :href="attachment.file_url" 
              target="_blank"
              class="flex-shrink-0 px-3 py-1.5 bg-[#01416c] hover:bg-[#026aa7] text-white text-sm rounded-lg transition"
            >
              Download
            </a>
          </div>
        </div>
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

const formatFileSize = (bytes) => {
  if (bytes === 0) return '0 Bytes'
  const k = 1024
  const sizes = ['Bytes', 'KB', 'MB', 'GB']
  const i = Math.floor(Math.log(bytes) / Math.log(k))
  return Math.round(bytes / Math.pow(k, i) * 100) / 100 + ' ' + sizes[i]
}

const formatDuration = (seconds) => {
  const mins = Math.floor(seconds / 60)
  const secs = seconds % 60
  return `${mins}:${secs.toString().padStart(2, '0')}`
}

const openMedia = (url) => {
  window.open(url, '_blank')
}
</script>
