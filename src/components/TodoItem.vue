<script setup>
defineProps({
    index: {
        type: Number
    },
    todo: {
        type: Object
    },
    editId: {
        type: Number
    },
})

const editTitle = defineModel('title', { required: true })

const emit = defineEmits(['checkCompleted', 'delete', 'editTodo', 'saveEdit', 'cancelEdit'])

const saveTodo = (todo) => {
    emit('saveEdit', todo)
}

</script>

<template>
    <!-- Edit Mode -->
    <template v-if="editId === todo.id">
        <div class="flex items-center w-full gap-2">
        <input 
            v-model="editTitle"
            @keyup.enter="saveTodo(todo)"
            @keyup.escape="$emit('cancelEdit')"
            class="flex-1 border-b-2 border-teal-500 focus:outline-none px-2 py-1 text-gray-700"
            type="text"
            autofocus
        />
        <!-- Save Button -->
        <button @click="$emit('saveEdit', todo)" class="p-1 text-green-600 hover:bg-green-100 rounded transition" title="Save">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
            </svg>
        </button>

        <!-- Cancel Button -->
        <button @click="$emit('cancelEdit')" class="p-1 text-gray-500 hover:bg-gray-200 rounded transition" title="Cancel">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
        </button>
        </div>
    </template>

        <!-- View Mode -->
    <template v-else>
        <div class="flex items-center">
        <input @input="$emit('checkCompleted', index)" :checked="todo.completed"
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
        <button @click="$emit('editTodo', todo)" class="p-1 text-blue-500 hover:text-blue-700 hover:bg-blue-50 rounded-full transition" title="Edit">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
            </svg>
        </button>

        <!-- Delete Button -->
        <button @click="$emit('delete', todo)" class="p-1 text-gray-400 hover:text-red-500 hover:bg-red-50 rounded-full transition" title="Delete">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
            </svg>
        </button>

        </div>
    </template>
</template>
