<script setup>
import { ref } from 'vue'
const emit = defineEmits(['addNewTodo'])
const newTodo = ref('')

function emitNewTodo() {
  emit('addNewTodo', newTodo.value)
  newTodo.value = ''
}
</script>

<template>
  <form @submit.prevent="emitNewTodo" class="new-todo-form">
    <input v-model="newTodo" class="new-todo" name="newTodo" placeholder="Еnter the task" />
    <button class="btn" :disabled="!newTodo.trim()">Add New ToDo</button>
  </form>
</template>

<style>
.new-todo-form {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.new-todo {
  width: 90%;
  height: 50px;
  font-size: 25px;
  padding: 10px;
  border-radius: 10px;
  border: 1px solid;
}
</style>
