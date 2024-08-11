<script>
import { reactive } from 'vue'

export default {
  setup() {
    const data = reactive({
      newTodo: '',
      todos: []
    })

    function addNewTodo() {
      data.todos.push({
        value: data.newTodo,
        isDone: false
      })
      data.newTodo = ''
      saveTodosToLocalStorage()
    }

    function removeTodo(index) {
      data.todos.splice(index, 1)
      saveTodosToLocalStorage()
    }

    function markDoneToggle(todo) {
      todo.isDone = !todo.isDone
      saveTodosToLocalStorage()
    }

    function markAllDoneToggle() {
      data.todos.every((todo) => todo.isDone)
        ? data.todos.forEach((todo) => (todo.isDone = false))
        : data.todos.forEach((todo) => (todo.isDone = true))
      saveTodosToLocalStorage()
    }

    function clearTodoList() {
      data.todos = []
      saveTodosToLocalStorage()
    }

    const storedTodos = localStorage.getItem('todos')
    if (storedTodos) {
      data.todos = JSON.parse(storedTodos)
    }

    function saveTodosToLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(data.todos))
    }

    return {
      data,
      addNewTodo,
      markDoneToggle,
      removeTodo,
      markAllDoneToggle,
      clearTodoList
    }
  }
}
</script>

<template>
  <div class="content">
    <header><h1>ToDo App</h1></header>
    <main>
      <form @submit.prevent="addNewTodo" class="new-todo-form">
        <input
          class="new-todo"
          v-model="data.newTodo"
          name="newTodo"
          placeholder="Еnter the task"
        />
        <button class="btn" :disabled="!data.newTodo.trim()">Add New ToDo</button>
      </form>
      <ul>
        <li v-for="(todo, index) in data.todos" :key="todo.id">
          <span :class="['todo-text', { done: todo.isDone }]">{{ todo.value }}</span>
          <div class="todo-right-side">
            <label :for="index" class="checkbox-label">
              <input
                class="checkbox"
                type="checkbox"
                :id="index"
                :checked="todo.isDone"
                @change="markDoneToggle(todo)"
              />
              <span class="checkbox-view">
                <svg
                  class="checkbox-icon"
                  xmlns="http://www.w3.org/2000/svg"
                  width="18"
                  viewBox="0 0 511.985 511.985"
                >
                  <path
                    fill="#fff"
                    d="M500.088 83.681c-15.841-15.862-41.564-15.852-57.426 0L184.205 342.148 69.332 227.276c-15.862-15.862-41.574-15.862-57.436 0-15.862 15.862-15.862 41.574 0 57.436l143.585 143.585c7.926 7.926 18.319 11.899 28.713 11.899 10.394 0 20.797-3.963 28.723-11.899l287.171-287.181c15.862-15.851 15.862-41.574 0-57.435z"
                  />
                </svg>
              </span>
            </label>
            <button class="cross-btn" @click="removeTodo(index)"></button>
          </div>
        </li>
      </ul>
      <div class="btn-container">
        <button class="btn" v-if="data.todos.length > 0" @click="markAllDoneToggle">
          {{
            data.todos.every((todo) => todo.isDone) ? `Mark all 'in progress'` : `Mark all 'done'`
          }}
        </button>
        <button class="btn" v-if="data.todos.length > 0" @click="clearTodoList">
          Clear todo list
        </button>
      </div>
    </main>
  </div>
</template>

<style scoped>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}
main {
  width: 90%;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

h1 {
  text-align: center;
  margin: 20px;
}

.new-todo-form {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.btn {
  border-radius: 10px;
  color: white;
  transition: 0.2s linear;
  background: #0b63f6;
  border: #0b63f6;
  width: 140px;
  height: 50px;
  padding: 5px;
}
@media (hover: hover) {
  .btn:hover {
    -webkit-transform: rotateZ(5deg);
    -ms-transform: rotateZ(5deg);
    transform: rotateZ(5deg);
    cursor: pointer;
  }
}

.btn:disabled {
  background-color: #ccc;
  pointer-events: none;
}

.new-todo {
  width: 90%;
  height: 50px;
  font-size: 25px;
  padding: 10px;
  border-radius: 10px;
  border: 1px solid;
}

ul {
  display: flex;
  flex-direction: column;
  list-style: none;
  width: 100%;
  gap: 20px;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  padding: 10px;
  border: 2px solid white;
  border-radius: 10px;
}

li:has(.done) {
  border: 2px solid green;
}

.todo-text {
  display: inline-block;
  vertical-align: middle;
  font-size: 25px;
  height: 30px;
  line-height: 30px;
}

.todo-right-side {
  display: flex;
  align-items: center;
  gap: 10px;
}

.checkbox {
  position: absolute;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}

.checkbox-label {
  cursor: pointer;
  display: flex;
  align-items: center;
  font-size: 18px;
}

.checkbox-view {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
  background: #fff;
  transition: 0.3s;
}

.checkbox-icon {
  opacity: 0;
  transition: 0.3s;
}

.checkbox:checked + .checkbox-view {
  background-color: green;
}

.checkbox:checked + .checkbox-view .checkbox-icon {
  opacity: 1;
}

.cross-btn {
  width: 24px;
  height: 24px;
  border: none;
  background-image: url('./icons/cross.svg');
  background-size: cover;
  cursor: pointer;
  border-radius: 20px;
}

.new-todo-form {
  display: flex;
  gap: 20px;
}

.done {
  text-decoration: line-through;
}

.btn-container {
  display: flex;
  justify-content: center;
  gap: 20px;
}
</style>
