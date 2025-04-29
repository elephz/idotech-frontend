<template>
  <div>
    <h1>Create Technician</h1>
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

const alert = ref({
  "show": false,
  "message": "",
  "type": ""
})

const formFields = [
  { label: 'Name', name: 'name', type: 'text', required: true },
  { label: 'Skills', name: 'skills', type: 'text', required: true },
]

const handleSubmit = async (formData) => {
  try {
    const payload = {
      name: formData.name,
      skills: formData.skills.split(',').map(skill => skill.trim()),
    }

    const response = await axios.post(
      `${import.meta.env.VITE_API_URL}/api/technicians`,
      payload,
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
