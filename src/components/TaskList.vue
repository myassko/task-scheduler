<template>
  <ul>
    <transition-group name="fade" tag="ul">
      <TaskItem
        v-for="task in tasks"
        :key="task.id"
        :task="task"
        :showCompleted="showCompleted"
        @toggle-done="$emit('toggle-done', task.id)"
        @remove-task="$emit('remove-task', task.id)"
        @edit-task="$emit('edit-task', task.id, $event)"
      />
    </transition-group>
  </ul>
</template>

<script setup>
import TaskItem from './TaskItem.vue'
defineProps(['tasks', 'showCompleted'])
defineEmits(['toggle-done', 'remove-task', 'edit-task'])
</script>

<style scoped>
.fade-enter-active, .fade-leave-active {
  transition: all 0.3s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>
