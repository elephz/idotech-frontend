<template>
  <div>
    <label for="user-select">Select User:</label>
    <select v-model="selectedUserId" id="user-select" class="form-select">
      <option value="" disabled>Select a user</option>
      <option v-for="user in users" :key="user.id" :value="user.id">
        {{ user.name }}
      </option>
    </select>

    <p class="mt-3">Selected User ID: {{ selectedUserId }}</p>
    <Form />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Form from '../components/Form.vue'

const users = ref([])
const selectedUserId = ref('')

onMounted(async () => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/users')
    users.value = await res.json()
  } catch (error) {
    console.error('Failed to fetch users:', error)
  }
})
</script>
