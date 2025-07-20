<template>
  <div class="p-4">
    <h1>📝 Мой планировщик задач</h1>

    <div style="margin-bottom: 10px;">
      <button @click="showCompleted = false">Актуальные</button>
      <button @click="showCompleted = true">Выполненные</button>
    </div>

    <div v-if="!showCompleted">
      <input v-model="newTask" @keyup.enter="addTask" placeholder="Новая задача" />
      <button @click="addTask">Добавить</button>
    </div>

    <TaskList
      :tasks="filteredTasks"
      :showCompleted="showCompleted"
      @toggle-done="toggleDone"
      @remove-task="removeTask"
      @edit-task="editTask"
    />
  </div>
</template>

<script setup>
import { ref, watch, onMounted, computed } from 'vue'
import TaskList from './components/TaskList.vue'

const tasks = ref([])
const newTask = ref('')
const showCompleted = ref(false)

onMounted(() => {
  tasks.value = JSON.parse(localStorage.getItem('tasks') || '[]')
})

watch(tasks, () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}, { deep: true })

function addTask() {
  if (newTask.value.trim()) {
    tasks.value.push({
      id: Date.now(),
      text: newTask.value,
      done: false
    })
    newTask.value = ''
  }
}

function toggleDone(id) {
  const task = tasks.value.find(t => t.id === id)
  if (task) task.done = !task.done
}

function removeTask(id) {
  tasks.value = tasks.value.filter(t => t.id !== id)
}

function editTask(id, newText) {
  const task = tasks.value.find(t => t.id === id)
  if (task) task.text = newText
}

const filteredTasks = computed(() =>
  tasks.value.filter(t => t.done === showCompleted.value)
)
</script>
