<template>
  <div class="bg-white px-5 py-3 border-b border-gray-200 flex gap-2 flex-wrap">
    <div v-for="participant in participants" :key="participant.id"
      class="inline-flex items-center gap-2 px-3 py-1.5 bg-gray-100 rounded-full text-sm">
      <span>{{ participant.name }}</span>
      <span :class="[
        'px-2 py-0.5 rounded-full text-xs font-semibold',
        participant.role === 0 ? 'bg-red-500 text-white' : '',
        participant.role === 1 ? 'bg-teal-500 text-white' : '',
        participant.role === 2 ? 'bg-green-400 text-gray-800' : ''
      ]">
        {{ getRoleLabel(participant.role) }}
      </span>
    </div>
  </div>
</template>

<script setup>
defineProps({
  participants: {
    type: Array,
    required: true
  }
})

const getRoleLabel = (role) => {
  const labels = { 0: 'Admin', 1: 'Agent', 2: 'Customer' }
  return labels[role] || 'User'
}
</script>
