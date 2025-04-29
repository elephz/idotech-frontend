<template>
  <div>
      <div class="d-flex justify-content-between align-items-center">
        <h1>Match Result</h1>
         <div class="mt-4">
          <h4>ใช้เวลาในการจัดตาราง {{ totalDays }} วัน</h4>
        </div>
      </div>

      <PlacheholerComponent  v-if="loading"/>
      <div v-else class="mt-4">
        <b class="my-5">รายการงานแต่ละชิ้น </b>
        <TableComponent :headers="taskHeaders" :data="taskData" class="mt-3" />
        <br>
        <b class="my-5">ตารางรายวัน </b>
        <TableComponent :headers="scheduleHeaders" :data="dailySchedule" class="mt-3" />
       
      </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import TableComponent from '../components/TableComponent.vue'
import PlacheholerComponent from '../components/PlacheholerComponent.vue'
import axios from 'axios'

const taskHeaders = [
  { key: 'task_title', label: 'งาน' },
  { key: 'start_date', label: 'วันที่เริ่ม' },
  { key: 'end_date', label: 'วันที่สิ้นสุด' },
  { key: 'technicians', label: 'ช่างที่รับผิดชอบ' }
]

const scheduleHeaders = [
  { key: 'date', label: 'วัน' },
  { key: 'technician', label: 'ช่าง' },
  { key: 'task', label: 'งานที่ทำ' },
]

const taskData = ref([])
const dailySchedule = ref([])
const totalDays = ref(0)
const loading = ref(true)

onMounted(async () => {
  try {
    loading.value = true
    const response = await axios.get(`${import.meta.env.VITE_API_URL}/api/match`)
    loading.value = false
    const data = response.data
  
    totalDays.value = data.total_days
    taskData.value = data.assigned_tasks.map(task => ({ ...task, technicians: task.technicians.join(', ') }))

    const schedule = []
    for (const task of data.assigned_tasks) {
      const start = new Date(task.start_date)
      const end = new Date(task.end_date)
      console.log({start, end})
      for (let d = new Date(start); d <= end; d.setDate(d.getDate() + 1)) {
        const dateStr = d.toISOString().slice(0, 10)
        task.technicians.forEach(tech => {
          schedule.push({ date: dateStr, technician: tech, task: task.task_title })
        })
      }
    }
    dailySchedule.value = schedule.sort((a, b) => a.date.localeCompare(b.date))

  } catch (error) {
    console.error(error)
  }
})
</script>
