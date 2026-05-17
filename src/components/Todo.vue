<script setup>
import { reactive, ref, computed } from 'vue'
import TodoItem from './TodoItem.vue'
import TodoForm from './TodoForm.vue'
import TodoHeader from './TodoHeader.vue'

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

const addTodo = (todo) => {
  todos.push({
    id: todos.length + 1,
    title: todo,
    completed: false
  })
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

    <TodoHeader :allTodos="allTodos" :completedTodos="completedTodos"/>

    <TodoForm @addTodo="addTodo" />

    <!-- Todo List -->
    <ul class="space-y-3" v-if="todos.length" >
      <li v-for="(todo, index) in todos" :key="todo.id" class="group flex items-center justify-between p-2 hover:bg-gray-50 rounded-lg transition-colors">
        
        <TodoItem 
          :index = "index"
          :todo = "todo"
          :editId = "editId"
          v-model:title="editTitle"
          @editTodo = "editTodo"
          @saveEdit = "saveEdit"
          @cancelEdit = "cancelEdit"
          @checkCompleted = "checkCompleted"
          @delete = "removeTodo"
        />

      </li>
    </ul>

    <p  v-else class="text-center text-gray-400">No todos found.  Add one above!</p>
    
  </div>
</template>