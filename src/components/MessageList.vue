<template>
  <div ref="messagesContainer" class="flex-1 min-h-0 overflow-y-auto p-5 bg-gray-50">
    <MessageBubble
      v-for="message in messages"
      :key="message.id"
      :message="message"
      :sender-name="getSenderName(message.sender)"
      :is-own="isSentByMe(message.sender)"
    />
  </div>
</template>

<script setup>
import { ref, watch, nextTick } from 'vue'
import MessageBubble from './MessageBubble.vue'

const props = defineProps({
  messages: {
    type: Array,
    required: true
  },
  participants: {
    type: Array,
    required: true
  },
  currentUser: {
    type: String,
    required: true
  }
})

const messagesContainer = ref(null)

const isSentByMe = (sender) => {
  return sender === props.currentUser
}

const getSenderName = (senderId) => {
  const participant = props.participants.find(p => p.id === senderId)
  return participant ? participant.name : senderId
}

const scrollToBottom = () => {
  if (messagesContainer.value) {
    messagesContainer.value.scrollTop = messagesContainer.value.scrollHeight
  }
}

watch(() => props.messages, () => {
  nextTick(() => {
    scrollToBottom()
  })
}, { deep: true })

defineExpose({
  scrollToBottom
})
</script>
