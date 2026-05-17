<script setup>
defineProps({
    allTodos: {
        type: Number
    },
    completedTodos: {
        type: Number
    }
})
</script>

<template>
  <!-- Header -->
    <h1 class="text-2xl uppercase font-bold text-gray-800 tracking-wide">
      To Do List <span v-if="allTodos" class="text-sm font-medium text-teal-600 bg-teal-50 px-2 py-1 rounded-full">{{ completedTodos }}/{{ allTodos }}</span>
    </h1>
</template>
