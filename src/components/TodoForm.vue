<script setup>
import { ref } from 'vue'

const todo = ref("")
const emit = defineEmits(['addTodo'])

const addTodo = () => {
  if (todo.value.trim() === "") {
    return
  }
  emit('addTodo', todo.value)
  todo.value = ""
}
</script>

<template>
    <!-- Form -->
    <form @submit.prevent="addTodo" class="relative">
      <div class="flex items-center border-b-2 border-teal-500 focus-within:border-teal-700 transition-colors pb-1">
        <input v-model="todo"
          class="border-none focus:outline-none w-full text-gray-700 placeholder-gray-400 py-2"
          type="text"
          placeholder="Add new task..."
        />
        <button
          class="shrink-0 bg-teal-500 hover:bg-teal-600 text-white text-sm font-semibold py-1.5 px-4 rounded-md shadow-sm transition-all duration-200 active:scale-95"
          type="submit"
        >
          Add
        </button>
      </div>
    </form>
    <!-- End Form -->
</template>
