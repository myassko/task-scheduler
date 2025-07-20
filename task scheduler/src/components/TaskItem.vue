<template>
  <li>
    <input type="checkbox" v-model="localDone" @change="emitToggleDone" />

    <span v-if="!editing" @dblclick="startEditing">
      {{ task.text }}
    </span>

    <input
      v-else
      v-model="editedText"
      @blur="saveEdit"
      @keyup.enter="saveEdit"
    />

    <button v-if="!showCompleted" @click="$emit('remove-task')">❌</button>
    <button v-if="showCompleted" @click="emitToggleDone">↩ Вернуть</button>
  </li>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps(['task', 'showCompleted'])
const emit = defineEmits(['toggle-done', 'remove-task', 'edit-task'])

const editing = ref(false)
const editedText = ref(props.task.text)
const localDone = ref(props.task.done)

watch(() => props.task.done, val => localDone.value = val)

function emitToggleDone() {
  emit('toggle-done')
}

function startEditing() {
  if (!props.task.done) {
    editedText.value = props.task.text
    editing.value = true
  }
}

function saveEdit() {
  emit('edit-task', editedText.value)
  editing.value = false
}
</script>
