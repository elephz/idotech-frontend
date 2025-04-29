<template>
  <form @submit.prevent="handleSubmit" class="p-4 border rounded">
    <div
      class="mb-3"
      v-for="(input, index) in inputs"
      :key="index"
    >
      <label class="form-label">{{ input.label }}</label>

      <!-- Select Field -->
      <select
        v-if="input.type === 'select'"
        v-model="form[input.name]"
        class="form-control"
        :required="input.required"
      >
        <option disabled value="">Please select</option>
        <option
          v-for="option in input.options"
          :key="option.value"
          :value="option.value"
        >
          {{ option.label }}
        </option>
      </select>

      <!-- Other Input Fields -->
      <input
        v-else
        v-model="form[input.name]"
        :type="input.type || 'text'"
        class="form-control"
        :required="input.required"
        :autocomplete="input.autocomplete || 'off'"
      />
    </div>

    <button type="submit" class="btn btn-primary">{{ submitText }}</button>
  </form>
</template>

<script setup>
import { reactive } from 'vue'
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  inputs: Array,
  submitText: {
    type: String,
    default: 'Submit',
  },
})

const emit = defineEmits(['submit'])

const form = reactive({})

// Initialize form fields
props.inputs.forEach(input => {
  form[input.name] = input.default ?? ''
})

const handleSubmit = () => {
  emit('submit', { ...form })

  // Reset form
  props.inputs.forEach(input => {
    form[input.name] = ''
  })
}
</script>
