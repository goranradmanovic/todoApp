<template>
  <div class="row">
    <div class="col-md-4 offset-md-4">
      <form>
        <div class="mt-5 mb-3">
          <label for="exampleInput1" class="form-label">Create new todo task</label>
          <input
            type="text"
            class="form-control"
            id="exampleInput1"
            aria-describedby="titleHelp"
            placeholder="Add new task"
            v-model="title"
          />
          <div id="titleHelp" class="form-text text-danger" v-for="error in v$.title.$errors" :key="error.$uid">
            {{ error.$message }}
          </div>
        </div>
        <button type="submit" class="btn btn-outline-secondary" @click.prevent="submitForm()">Create Task</button>
      </form>
      <hr />
    </div>
  </div>
</template>

<script setup>
import { useVuelidate } from '@vuelidate/core'
import { required, } from '@vuelidate/validators'
import { ref } from 'vue'

const emit = defineEmits(['create-task'])

// Variables
const title = ref(''),
      rules = {
        title: { required },
      },
      v$ = useVuelidate(rules, { title })

// Functions
const submitForm = async () => {
  const result = await v$.value.$validate() // true/false

  if (result) {
    // success emit
    emit('create-task', title.value)
    title.value = ''
    return
  }
  
  // Form has failed
  return
}
</script>
