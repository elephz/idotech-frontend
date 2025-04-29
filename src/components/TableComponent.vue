<template>
  <div>
    <table class="table table-bordered">
      <thead>
        <tr>
          <th v-for="(header, index) in headers" :key="index">
            {{ header.label }}
          </th>
          <th v-if="hasActions">การกระทำ</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, rowIndex) in data" :key="rowIndex">
          <td v-for="(header, colIndex) in headers" :key="colIndex">
            {{ getNestedValue(row, header.key) }}
          </td>
          <td v-if="hasActions">
            <button
              v-for="action in actions"
              :key="action.name"
              class="btn btn-sm me-1"
              :class="'btn-' + (action.type || 'primary')"
              @click="$emit('action', { name: action.name, row })"
            >
              {{ action.label }}
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  headers: Array,   // [{ key: 'task_title', label: 'งาน' }, ...]
  data: Array,
  actions: {
    type: Array,
    default: () => [] // [{ name: 'view', label: 'ดู', type: 'info' }]
  }
})
console.log({props : props.data})
const hasActions = !!props.actions?.length

// รองรับ key แบบ nested เช่น "user.name" หรือ "technicians[0]"
function getNestedValue(obj, path) {
  try {
    return path.split(/\.|\[(\d+)\]/).filter(Boolean).reduce((o, k) => o[k], obj)
  } catch (e) {
    return ''
  }
}
</script>
