<template>
  <li>
    <input
      class="form-check-input me-1"
      type="checkbox"
      value=""
      :id="props.todoItem.id"
      v-model="isCompleted"
      :disabled="props.todoItem.isEditing"
    />

    <label v-if="!props.todoItem.isEditing" class="form-check-label stretched-link text-start lead flex-grow-1 ps-2" :for="props.todoItem.id" editable>
      {{ props.todoItem.title }}
    </label>
    <input v-else type="text" class="form-control mx-3" v-model="todoTitle">

    <span class="d-inline-flex justify-content-between align-items-center p-1 todo-buttons-group">
      <svg v-if="props.todoItem.isEditing" @click="editTodo" xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-check-circle text-success"><path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/><path d="M10.97 4.97a.235.235 0 0 0-.02.022L7.477 9.417 5.384 7.323a.75.75 0 0 0-1.06 1.06L6.97 11.03a.75.75 0 0 0 1.079-.02l3.992-4.99a.75.75 0 0 0-1.071-1.05z"/></svg>
      <svg v-else @click="editTodo" xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil-square text-secondary"><path d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z"/><path fill-rule="evenodd" d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z"/></svg>
      <svg @click="deleteTodo" xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash3 text-danger"><path d="M6.5 1h3a.5.5 0 0 1 .5.5v1H6v-1a.5.5 0 0 1 .5-.5ZM11 2.5v-1A1.5 1.5 0 0 0 9.5 0h-3A1.5 1.5 0 0 0 5 1.5v1H2.506a.58.58 0 0 0-.01 0H1.5a.5.5 0 0 0 0 1h.538l.853 10.66A2 2 0 0 0 4.885 16h6.23a2 2 0 0 0 1.994-1.84l.853-10.66h.538a.5.5 0 0 0 0-1h-.995a.59.59 0 0 0-.01 0H11Zm1.958 1-.846 10.58a1 1 0 0 1-.997.92h-6.23a1 1 0 0 1-.997-.92L3.042 3.5h9.916Zm-7.487 1a.5.5 0 0 1 .528.47l.5 8.5a.5.5 0 0 1-.998.06L5 5.03a.5.5 0 0 1 .47-.53Zm5.058 0a.5.5 0 0 1 .47.53l-.5 8.5a.5.5 0 1 1-.998-.06l.5-8.5a.5.5 0 0 1 .528-.47ZM8 4.5a.5.5 0 0 1 .5.5v8.5a.5.5 0 0 1-1 0V5a.5.5 0 0 1 .5-.5Z"/></svg>
    </span>
  </li>
</template>

<script setup>
import { computed } from 'vue'

const emit = defineEmits(['toggle-complete', 'todo-edit', 'update-todotitle', 'delete-todo'])
const props = defineProps({
  todoItem: {
    type: Object,
    required: true,
    default: () => {}
  },
  index: {
    type: Number,
    required: true
  }
})

const editTodo = () => {
  emit('edit-todo', props.index)
}

const deleteTodo = () => {
  emit('delete-todo', props.todoItem.id)
}

const isCompleted = computed({
  get() {
    return props.todoItem.isCompleted
  },
  set() {
    emit('toggle-complete', props.index)
  }
})

const todoTitle = computed({
  get() {
    return props.todoItem.title
  },
  set(value) {
    emit('update-todotitle', value, props.index)
  }
})
</script>

