<script setup>
import { reactive, ref, computed } from 'vue'

const todo = ref("")

const todos = reactive([
  { id: 1, title: 'Todo 1', completed: false },
  { id: 2, title: 'Todo 2', completed: true },
  { id: 3, title: 'Todo 3', completed: false },
])

const checkCompleted = (index) => {
  todos[index].completed = !todos[index].completed
}

const removeTodo = (index) => {
  todos.splice(index, 1)
}

const addTodo = () => {
  if(todo.value.trim() === "") {
    return
  }

  todos.push({
    id: todos.length + 1,
    title: todo.value,
    completed: false
  })

  todo.value = ""  
}

const allTodos = computed(() => {
  return todos.length
})

const completedTodos = computed(() => {
  return todos.filter(todo => todo.completed).length
})

</script>

<template>
  <div class="max-w-md mx-auto bg-white shadow-xl rounded-xl overflow-hidden mt-10 p-6 space-y-6 border border-gray-100">

    <!-- Header -->
    <h1 class="text-2xl uppercase font-bold text-gray-800 tracking-wide">
      To Do List <span v-if="todos.length" class="text-sm font-medium text-teal-600 bg-teal-50 px-2 py-1 rounded-full">{{ completedTodos }}/{{ allTodos }}</span>
    </h1>

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

    <!-- Todo List -->
    <ul class="space-y-3" v-if="todos.length" >
      <li v-for="(todo, index) in todos" :key="todo.id" class="group flex items-center justify-between p-2 hover:bg-gray-50 rounded-lg transition-colors">
        <div class="flex items-center">
          <input @input="checkCompleted(index)" :checked="todo.completed"
            class="h-5 w-5 text-teal-600 focus:ring-teal-500 border-gray-300 rounded cursor-pointer"
            type="checkbox" :id="`todo-${todo.id}`"
          />
          <label class="ml-3 block text-gray-700 cursor-pointer select-none group-hover:text-gray-900" :for="`todo-${todo.id}`">
            <span :class="{'line-through text-gray-400': todo.completed}" class="text-lg font-medium transition-all">{{ todo.title }}</span>
          </label>
        </div>

        <button @click="removeTodo(index)" class="shrink-0 p-1 text-gray-400 hover:text-red-500 hover:bg-red-50 rounded-full transition-all">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5">
            <path stroke-linecap="round" stroke-linejoin="round" d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0" />
          </svg>
        </button>
      </li>
    </ul>

    <p v-else="!todos.length" class="text-center text-gray-400">No todos found.  Add one above!</p>
    
  </div>
</template>