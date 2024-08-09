<script>
import { reactive } from 'vue'

export default {
  setup() {
    const data = reactive({
      newTodo: '',
      id: 0,
      todos: []
    })

    function addNewTodo() {
      data.todos.push({
        value: data.newTodo,
        isDone: false
      })
      data.id++
      data.newTodo = ''
      saveTodosToLocalStorage();
    }

    function removeTodo(index) {
      data.todos.splice(index, 1)
      saveTodosToLocalStorage();
    }

    function markDoneToggle(todo) {
      todo.isDone = !todo.isDone
      saveTodosToLocalStorage();
    }

    function markAllDoneToggle() {
      data.todos.every((todo) => todo.isDone)
        ? data.todos.forEach((todo) => (todo.isDone = false))
        : data.todos.forEach((todo) => (todo.isDone = true))
        saveTodosToLocalStorage();
    }

    function clearTodoList() {
      data.todos = []
      saveTodosToLocalStorage();
    }

    const storedTodos = localStorage.getItem('todos');
    if (storedTodos) {
      data.todos = JSON.parse(storedTodos);
    }

    function saveTodosToLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(data.todos));
    }

    return {
      data,
      addNewTodo,
      markDoneToggle,
      removeTodo,
      markAllDoneToggle,
      clearTodoList,
    }
  }
}
</script>

<template>
  <h1>ToDo App</h1>
  <form @submit.prevent="addNewTodo">
    <input v-model="data.newTodo" name="newTodo" placeholder="Еnter the task" />
    <button>Add New ToDo</button>
  </form>
  <ul v-for="(todo, index) in data.todos" :key="todo.id">
    <li>
      <span :class="{ done: todo.isDone }">{{ todo.value }}</span>
      <button class="btn" @click="markDoneToggle(todo)">
        {{ todo.isDone ? `Mark 'in progress'` : `Mark 'done'` }}
      </button>
      <button class="btn" @click="removeTodo(index)">Remove Todo</button>
    </li>
  </ul>
  <button v-if="data.todos.length > 0" @click="markAllDoneToggle">
    {{ data.todos.every((todo) => todo.isDone) ? `Mark all 'in progress'` : `Mark all 'done'` }}
  </button>
  <button v-if="data.todos.length > 0" @click="clearTodoList">Clear todo list</button>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}

.btn {
  cursor: pointer;
}
</style>
