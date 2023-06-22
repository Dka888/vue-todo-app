<template>
    <div>
      <h2 class="todo-heading">Todo App</h2>
      <form v-on:submit.prevent="addTodo" class="todo-form">
        <input type="text" v-model="newTodo" placeholder="Add a new todo" required class="todo-input">
        <button type="submit" class="todo-button">Add</button>
      </form>
      <ul class="todo-list">
        <li v-for="todo in todos" :key="todo.id" class="todo-item">
          <input type="text" v-model="todo.title" @change="updateTodo(todo)" class="todo-item-input">
          <input type="checkbox" :checked="todo.completed" @change="toggleTodoStatus(todo)" class="todo-checkbox">
          <button v-on:click="removeTodo(todo.id)" class="todo-item-button">X</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        todos: [],
        newTodo: '',
      };
    },
    mounted() {
      this.fetchTodos();
    },
    methods: {
      fetchTodos() {
        axios
          .get('https://jsonplaceholder.typicode.com/todos?_limit=5')
          .then((response) => {
            this.todos = response.data;
          })
          .catch((error) => {
            console.error(error);
          });
      },
      addTodo() {
        if (this.newTodo !== '') {
          const newTodo = {
            id: this.todos.length + 1,
            title: this.newTodo,
            completed: false,
          };
          this.todos.push(newTodo);
          this.newTodo = '';
        }
      },
      toggleTodoStatus(todo) {
      todo.completed = !todo.completed;
      axios
        .put(`https://jsonplaceholder.typicode.com/todos/${todo.id}`, todo)
        .then(() => {
          console.log('Todo status updated successfully!');
        })
        .catch((error) => {
          console.error(error);
        });
    },
      updateTodo(todo) {
        axios
          .put(`https://jsonplaceholder.typicode.com/todos/${todo.id}`, todo)
          .then(() => {
            console.log('Todo updated successfully!');
          })
          .catch((error) => {
            console.error(error);
          });
      },
      removeTodo(todoId) {
        this.todos = this.todos.filter((todo) => todo.id !== todoId);
      },
    },
  };
  </script>
  
 
  
  <style>
  .todo-heading {
    font-size: 24px;
    margin-bottom: 16px;
  }
  
  .todo-form {
    margin-bottom: 16px;
  }
  
  .todo-input {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  .todo-button {
    padding: 8px 16px;
    background-color: #4caf50;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .todo-list {
    list-style-type: none;
    padding: 0;
  }
  
  .todo-item {
    display: flex;
    align-items: center;
    margin-bottom: 8px;
  }
  
  .todo-item-input {
    flex-grow: 1;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  .todo-item-button {
    margin-left: 8px;
    padding: 8px;
    background-color: #ff0000;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  </style>
  