<template>
  <div id="app">
    <TodoApp />
    <!-- <TodoError /> -->
  </div>
</template>

<script>
import TodoApp from './views/TodoApp.vue';
// import TodoError from './components/TodoError.vue';

export default {
  name: 'App',
  components: {
    TodoApp,
    // TodoError,
  },
};
</script>

