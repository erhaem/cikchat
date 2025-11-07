<template>
  <div class="h-screen min-h-0 flex flex-col max-w-full mx-auto bg-white">
    <template v-if="room && messages">
      <ChatHeader :room="room" />
      <ParticipantList :participants="room.participant" />
      <MessageList 
        class="flex-1 min-h-0"
        ref="messageListRef"
        :messages="messages" 
        :participants="room.participant"
        :current-user="currentUser"
      />
      <ChatInput 
        @send-message="handleSendMessage"
        @attach-file="handleAttachFile"
      />
    </template>
    <div v-else class="flex-1 flex items-center justify-center">
      <div class="text-gray-500">Loading chat...</div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue'
import ChatHeader from './components/ChatHeader.vue'
import ParticipantList from './components/ParticipantList.vue'
import MessageList from './components/MessageList.vue'
import ChatInput from './components/ChatInput.vue'

const room = ref(null)
const messages = ref(null)
const currentUser = ref('customer@mail.com')
const nextMessageId = ref(0)
const messageListRef = ref(null)

const loadChatData = async () => {
  try {
    const file = 'chat_response.json'
    const response = await fetch(`${import.meta.env.BASE_URL}${file}`)
    const data = await response.json()
    room.value = data.results[0].room
    messages.value = data.results[0].comments
    nextMessageId.value = messages.value.reduce((maxId, msg) => {
      return msg.id > maxId ? msg.id : maxId
    }, 0) // increment of latest mesesage ID
    
    nextTick(() => {
      if (messageListRef.value) {
        messageListRef.value.scrollToBottom()
      }
    })
  } catch (error) {
    console.error('failed loading chat data:', error)
  }
}

const handleSendMessage = (messageText) => {
  if (!messageText.trim()) return

  const newId = nextMessageId.value + 1
  nextMessageId.value = newId

  const newMessage = {
    id: newId,
    type: 'text',
    message: messageText,
    sender: currentUser.value
  }

  messages.value.push(newMessage)

  nextTick(() => {
    if (messageListRef.value) {
      messageListRef.value.scrollToBottom()
    }
  })
}

const handleAttachFile = (file) => {
  if (!file) return

  const mime = file.type || ''
  let type = 'file'

  if (mime.startsWith('image/')) type = 'image'
  else if (mime.startsWith('video/')) type = 'video'
  else if (mime === 'application/pdf') type = 'pdf'

  const newId = nextMessageId.value + 1
  nextMessageId.value = newId

  const attachment = {
    file_url: URL.createObjectURL(file),
    file_type: mime,
    file_name: file.name,
    file_size: file.size
  }

  if (type === 'video') {
    attachment.thumbnail_url = attachment.file_url
  }

  const newMessage = {
    id: newId,
    type,
    message: '',
    sender: currentUser.value,
    attachments: [attachment]
  }

  messages.value.push(newMessage)

  nextTick(() => {
    if (messageListRef.value) {
      messageListRef.value.scrollToBottom()
    }
  })
}

onMounted(() => {
  loadChatData()
})
</script>
