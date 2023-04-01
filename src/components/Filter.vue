<template>
  <div class="row">
    <div class="col-md-4 offset-md-4">
        <div>
            <h4>Filters</h4>
            <button type="button" class="btn btn-outline-secondary me-2" @click="$emit('all-todo')">All Items</button>
            <button type="button" class="btn btn-outline-secondary" @click="$emit('completed-todo')">Completed Items</button>
        </div>
        <hr />
    </div>
  </div>
</template>

<script setup>
defineEmits(['all-todo', 'completed-todo'])
</script>
