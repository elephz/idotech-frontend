<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light shadow-sm">
    <div class="container">
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item" v-for="item in navItems" :key="item.name">
            <router-link
              class="nav-link"
              :to="item.path"
              :aria-current="item.path === $route.path ? 'page' : null"
            >
              {{ item.name }}
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { useRoute } from 'vue-router'

const route = useRoute()

const navItems = [
  { name: 'Technician', path: '/technician' },
  { name: 'Task', path: '/task' },
  { name: 'Match Tasks', path: '/match' },
]
</script>
