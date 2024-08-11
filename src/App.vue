<script setup>
import TodoApp from './components/TodoApp.vue'
</script>

<template>
  <div class="content">
    <header><h1>ToDo App</h1></header>
    <main>
      <TodoApp />
    </main>
  </div>
</template>

<style>
body {
  display: flex;
  justify-content: center;
  max-width: 1900px;
}
#app {
  display: flex;
  justify-content: center;
  width: 100%;
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
</style>
