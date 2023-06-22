<template>
  <div id="app">
    <h1>Todo App</h1>
    <TodoApp />
  </div>
</template>

<script>
import TodoApp from './views/TodoApp.vue';

export default {
  name: 'App',
  components: {
    TodoApp,
  },
};
</script>

