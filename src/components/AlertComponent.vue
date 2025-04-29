<template>
  <div
    v-if="visible"
    :class="['alert', alertClass, 'alert-dismissible', 'fade', 'show']"
    role="alert"
  >
    <strong>{{ title }}</strong> {{ message }}
    <button
      type="button"
      class="btn-close"
      @click="visible = false"
      aria-label="Close"
    ></button>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'
const props = defineProps({
  type: {
    type: String,
    default: 'success', // 'success' or 'danger'
  },
  message: {
    type: String,
    required: true,
  },
})

const visible = ref(true)

const alertClass = computed(() => {
  return `alert-${props.type}`
})

const title = computed(() => {
  return props.type === 'success' ? 'Success!' : 'Error!'
})
</script>
