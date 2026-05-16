<script setup>
import { reactive, ref, computed } from 'vue'

const todo = ref("")
const editId = ref(null)
const editTitle = ref(null)

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
  if (todo.value.trim() === "") {
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

const editTodo = (todo) => {
  editId.value = todo.id
  editTitle.value = todo.title
}

const saveEdit = (todo) => {
  if (editTitle.value.trim() === "") {
    editId.value = null
    return
  }
  todo.title = editTitle.value.trim()
  editId.value = null
  editTitle.value = null
}

const cancelEdit = () => {
  editId.value = null
  editTitle.value = null
}

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
        
        <!-- Edit Mode -->
        <template v-if="editId === todo.id">
          <div class="flex items-center w-full gap-2">
            <input 
              v-model="editTitle"
              @keyup.enter="saveEdit(todo)"
              @keyup.escape="cancelEdit"
              class="flex-1 border-b-2 border-teal-500 focus:outline-none px-2 py-1 text-gray-700"
              type="text"
              autofocus
            />
            <!-- Save Button -->
            <button @click="saveEdit(todo)" class="p-1 text-green-600 hover:bg-green-100 rounded transition" title="Save">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
              </svg>
            </button>

            <!-- Cancel Button -->
            <button @click="cancelEdit()" class="p-1 text-gray-500 hover:bg-gray-200 rounded transition" title="Cancel">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
        </template>

          <!-- View Mode -->
        <template v-else>
          <div class="flex items-center">
            <input @input="checkCompleted(index)" :checked="todo.completed"
              class="h-5 w-5 text-teal-600 focus:ring-teal-500 border-gray-300 rounded cursor-pointer"
              type="checkbox" :id="`todo-${todo.id}`"
            />
            <label class="ml-3 block text-gray-700 cursor-pointer select-none group-hover:text-gray-900" :for="`todo-${todo.id}`">
              <span :class="{'line-through text-gray-400': todo.completed}" class="text-lg font-medium transition-all">{{ todo.title }}</span>
            </label>
          </div>

          <!-- Action Buttons -->
          <div class="flex items-center gap-1 opacity-100 sm:opacity-0 group-hover:opacity-100 transition-opacity">
           
            <!-- Edit Button -->
            <button @click="editTodo(todo)" class="p-1 text-blue-500 hover:text-blue-700 hover:bg-blue-50 rounded-full transition" title="Edit">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
              </svg>
            </button>

            <!-- Delete Button -->
            <button @click="removeTodo(index)" class="p-1 text-gray-400 hover:text-red-500 hover:bg-red-50 rounded-full transition" title="Delete">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
              </svg>
            </button>

          </div>
        </template>

      </li>
    </ul>

    <p v-else="!todos.length" class="text-center text-gray-400">No todos found.  Add one above!</p>
    
  </div>
</template>