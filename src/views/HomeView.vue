<template>
  <main>
    <CreateForm @create-task="createTask" />

    <Filter @all-todo="allTodo" @completed-todo="completedTodo" />

    <div class="row">
      <div class="col-md-4 offset-md-4">
        <div class="card mt-5">
          <div class="card-body">
            <h2 class="card-title">Todo list</h2>
              <TransitionGroup v-if="todoList.length > 0" class="list-group mt-5" name="list" tag="ul">
                <TodoItem
                  v-for="(item, index) in todoList" 
                  :key="item.id"
                  :todoItem="item" 
                  :index="index"
                  class="list-group-item d-flex justify-content-between align-items-center" 
                  :class="{ 'text-bg-info todo-completed': item.isCompleted }"
                  @toggle-complete="toggleTodoComplete"
                  @edit-todo="toggleTodoEdit"
                  @update-todotitle="updateTodoTitle"
                  @delete-todo="deleteTodo"
                />
              </TransitionGroup>
            <h6 v-else class="card-subtitle mb-2 text-body-secondary">There is no todo items</h6>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref, watch } from 'vue'
import { uid } from 'uid'

// Components
import CreateForm from '@/components/CreateForm.vue'
import TodoItem from '@/components/TodoItem.vue'
import Filter from '@/components/Filter.vue'

const todoList = ref([])
let isSetToLocalStorage = true

watch(todoList, () => {
  if (isSetToLocalStorage) {
    setToDoListLocalStorage()
  }
}, { deep: true })

const fetchToDoList = () => {
  const savedToDoList = JSON.parse(localStorage.getItem('todoList'))

  if (savedToDoList) {
    todoList.value = savedToDoList
  }
}

fetchToDoList()

const setToDoListLocalStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value))
}

// Functions
const createTask = (title) => {
  todoList.value.push({
    id: uid(),
    title,
    isCompleted: null,
    isEditing: null
  })
}

const toggleTodoComplete = (index) => {
  todoList.value[index].isCompleted = !todoList.value[index].isCompleted
}

const toggleTodoEdit = (index) => {
  todoList.value[index].isEditing = !todoList.value[index].isEditing
}

const updateTodoTitle = (value, index) => {
  todoList.value[index].title = value
}

const deleteTodo = (todoId) => {
  todoList.value = todoList.value.filter((item) => item.id !== todoId)
}

// Filters
const allTodo = () => {
  isSetToLocalStorage = true
  fetchToDoList()
}

const completedTodo = () => {
  isSetToLocalStorage = false
  todoList.value = todoList.value.filter(item => item.isCompleted)
}
</script>