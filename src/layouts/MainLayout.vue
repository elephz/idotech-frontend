<template>
  <div class="d-flex">
    <div class="flex-grow-1">
      <Navbar />
      <div class="p-4 container">
        <RouterView />
      </div>
    </div>
  </div>
</template>

<script setup>
import Navbar from '../components/Navbar.vue'
import { RouterView } from 'vue-router'
</script>
