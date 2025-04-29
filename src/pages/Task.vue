<template>
  <div>
    <h1>Create Task</h1>
    <div>
       <AlertComponent v-if="alert.show" :type="alert.type" :message="alert.message" />
    </div>
    <Form 
      :inputs="formFields"
      submit-text="Save"
      @submit="handleSubmit"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Form from '../components/Form.vue'
import axios from 'axios'
import AlertComponent from '../components/AlertComponent.vue'
const users = ref([])
const selectedUserId = ref('')

const alert = ref({
  "show": false,
  "message": "",
  "type": ""
})

const options = [
  { label: 'High', value: 'High' },
  { label: 'Medium', value: 'Medium' },
  { label: 'Low', value: 'Low' },
];

const formFields = [
  { label: 'Title', name: 'title', type: 'text', required: true },
  { label: 'Skill', name: 'required_skill', type: 'text', required: true },
  { label: 'Urgency', name: 'urgency', type: 'select', required: true, options, default : options[0].value },
  { label: 'Duration', name: 'duration', type: 'number', required: true, default : 1 },
  { label: 'Technicians', name: 'required_technicians', type: 'number', required: true,  default : 1 },
]

const handleSubmit = async (formData) => {
  try {
    const response = await axios.post(
      `${import.meta.env.VITE_API_URL}/api/task`,
      formData,
      {
        headers: {
          'Content-Type': 'application/json',
        },
      }
    )
    alert.value.type = "success"
    alert.value.message = "create task success"
    alert.value.show = true
  } catch (error) {
    alert.value.type = "danger"
    alert.value.message = "create task failed"
    alert.value.show = true
    console.error(error)
  }
}
</script>
