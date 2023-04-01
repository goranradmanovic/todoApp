<script setup>
import { RouterView } from 'vue-router'
import Header from '@/components/Header.vue'
</script>

<template>
  <div>
    <div class="container-fluid">
      <Header />
        
      <RouterView />
    </div>
  </div>
</template>
