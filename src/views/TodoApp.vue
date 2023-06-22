<!-- <template>
      <div class="todo">
        <h2 class="todo-heading">Todo App</h2>
        <div class="todo-container">
        <form v-on:submit.prevent="addTodo" class="todo-form">
          <input type="text" v-model="newTodo" placeholder="Add a new todo" required class="todo-input">
        </form>
        <ul class="todo-list">
          <li v-for="todo in todos" :key="todo.id" class="todo-item">
            <input type="checkbox" :checked="todo.completed" @change="toggleTodoStatus(todo)" class="todo-checkbox">
              <input type="text" v-model="todo.title" @change="updateTodo(todo)" class="todo-item-input" v-bind:class="{done: todo.completed}"> 
            <button v-on:click="removeTodo(todo.id)" class="todo-item-button">X</button>
          </li>
        </ul>
      </div>
      <footer class="todoapp__footer">
      <span class="todo-count">
      </span>
      <nav class="filter">
    <a
      href="#/"
      :class="{ 'filter__link selected': filtered === Filters.All, 'filter__link': filtered !== Filters.All }"
      @click="setFiltered(Filters.All)"
    >
      All
    </a>

    <a
      href="#/active"
      :class="{ 'filter__link selected': filtered === Filters.Active, 'filter__link': filtered !== Filters.Active }"
      @click="setFiltered(Filters.Active)"
    >
      Active
    </a>

    <a
      href="#/completed"
      :class="{ 'filter__link selected': filtered === Filters.Completed, 'filter__link': filtered !== Filters.Completed }"
      @click="setFiltered(Filters.Completed)"
    >
      Completed
    </a>
    </nav>

      <button
        type="button"
        class="todoapp__clear-completed"
      >
        Clear completed
      </button>

    </footer>
    </div>
  </template>
  
<script>
import axios from 'axios';

export default {
  data() {
    return {
      todos: [],
      newTodo: '',
      Filters: {
        All: 'All',
        Active: 'Active',
        Completed: 'Completed'
      },
      filtered: 'All'
    };
  },
  mounted() {
    this.fetchTodos();
  },
  methods: {
    fetchTodos() {
      axios
        .get('https://jsonplaceholder.typicode.com/todos?_limit=3')
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
    setFiltered(filter) {
      this.filtered = filter;
      // switch(filter) {
      //   case 'Active': 
      //     this.todos = this.todos.filter(todo => todo.completed !== todo.completed);
      //     break;
      //   case 'Completed': 
      //     return this.todos = this.todos.filter(todo => todo.completed === todo.completed);
      //   case 'All': 
      //     return this.todos;
      //   default: return this.todos;
      // }
    }
  },
};
</script>
  
 
  
<style>
  .todo {
    width: 400px;
    margin:auto;
    text-align: center;

  }
  .todo-container {
    box-shadow: 0 0 35px rgb(189, 192, 196);
    border: 2px solid rgb(175, 176, 179);
  }

  .todo-heading {
    font-size: 36px;
    text-align: center;
    padding-top: 100px;
    margin:auto;
    color: darkred;
    margin-bottom: 15px;
  }
  
  .todo-form {
    width: 100%;
    margin:auto;
  }
  
  .todo-input {
    width: 390px;
    height: 50px;
    border: transparent;
  }

  
  .todo-list {
    list-style-type: none;
    padding: 0;
    width: 100%;
    margin:auto;
  }
  
  .todo-item {
    display: flex;
    align-items: center;
    border: 1px solid #ccc;
  }
  
  .todo-item-input {
    flex-grow: 1;
    padding: 8px;
    border: 1px transparent;
  }
  
  .todo-item-button {
    margin: 4px;
    margin-left: 8px;
    padding: 8px;
    background-color:#fff;
    color: darkred;
    border: transparent;
    cursor: pointer;
  }
  .done {
    text-decoration: line-through;
  }

  .todoapp__footer {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }
  </style> -->
  

<template>
  <div class="todo">
    <h2 class="todo-heading">Todo App</h2>
    <div class="todo-container">
      <header>
        <button type="button" class="header-button" @click="toggleComplete">^</button>
        <form @submit.prevent="addTodo" class="todo-form">
          <input type="text" v-model="newTodo" placeholder="Add a new todo" required class="todo-input">
        </form>
      </header>
      <ul class="todo-list">
        <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
          <input type="checkbox" v-model="todo.completed" @change="toggleTodoStatus(todo)" class="todo-checkbox">
          <input type="text" v-model="todo.title" @change="updateTodo(todo)" :class="{ done: todo.completed }"
            class="todo-item-input">
          <button @click="removeTodo(todo.id)" class="todo-item-button">X</button>
        </li>
      </ul>
    
    <footer class="todoapp__footer">
        <span class="todo-count">{{ this.todos.filter(todo => !todo.completed).length }} items left</span>
        <nav class="filter">
          <a href="#/"
            :class="{ 'filter__link selected': filtered === Filters.All, 'filter__link': filtered !== Filters.All }"
            @click="setFiltered(Filters.All)"> All </a>
          <a href="#/active"
            :class="{ 'filter__link selected': filtered === Filters.Active, 'filter__link': filtered !== Filters.Active }"
            @click="setFiltered(Filters.Active)"> Active </a>
          <a href="#/completed"
            :class="{ 'filter__link selected': filtered === Filters.Completed, 'filter__link': filtered !== Filters.Completed }"
            @click="setFiltered(Filters.Completed)"> Completed </a>
        </nav>
        <button type="button" class="todoapp__clear-completed" @click="removeAllCompleted">Clear completed</button>
      </footer>
    </div>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
  data() {
    return {
      dataTodo: {
        headers: {
          'Content-Type': 'application/json'
        }
      },
      todos: [],
      newTodo: '',
      Filters: {
        All: 'All',
        Active: 'Active',
        Completed: 'Completed'
      },
      filtered: 'All'
    };
  },
  computed: {
    filteredTodos() {

      let newTodos = this.todos;
      console.log(this.todos, newTodos);
      switch (this.filtered) {
        case 'Active':
          newTodos = newTodos.filter(todo => !todo.completed);
          break;
        case 'Completed':
          newTodos = newTodos.filter(todo => todo.completed);
          break;
        default:
          newTodos = this.todos;
          break;
      }

      return newTodos;
    }
  },

  mounted() {
    this.fetchTodos();
  },
  methods: {
    fetchTodos() {
      axios
        .get('https://mate.academy/students-api/todos?userId=10529')
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
          id: 0,
          title: this.newTodo,
          completed: false,
          userId: 10529,
        };
        axios
          .post('https://mate.academy/students-api/todos?userId=10529', newTodo, this.dataTodo)
          .catch(error => console.log(error))
          .finally(() => {
            this.newTodo = '';
            this.fetchTodos();
          });
      }
    },
    toggleTodoStatus(todo) {
      let newTodo;
      if (this.todos.find(tod => !tod.completed)) {
        newTodo = {
          id: todo.id,
          title: todo.title,
          userId: todo.userId,
          completed: true,
        };
      } else {
        newTodo = {
          id: todo.id,
          title: todo.title,
          userId: todo.userId,
          completed: false,
        };
      }
      const data = {
        completed: newTodo.completed,
      };

      axios
        .patch(`https://mate.academy/students-api/todos/${newTodo.id}?userId=10529`, data, this.dataTodo)
        .then(() => {
          console.log('Todo status updated successfully!');
        })
        .catch((error) => {
          console.error(error);
        });
    },

    toggleComplete() {
      const isAllCompleted = this.todos.some(todo => Boolean(!todo.completed))
      if (isAllCompleted) {
        this.todos.forEach(todo => {
          const data = {
            completed: true,
          };

          axios
            .patch(`https://mate.academy/students-api/todos/${todo.id}?userId=10529`, data, this.dataTodo)
            .then(() => {
              console.log('Todo status updated successfully!');
            })
            .catch((error) => {
              console.error(error);
            })
            .finally(this.fetchTodos());
        })
      } else {
        this.todos.forEach(todo => {
          const data = {
            completed: false,
          };

          axios
            .patch(`https://mate.academy/students-api/todos/${todo.id}?userId=10529`, data, this.dataTodo)
            .then(() => {
              console.log('Todo status updated successfully!');
            })
            .catch((error) => {
              console.error(error);
            })
            .finally(this.fetchTodos());
        })
      }

    },

    updateTodo(todo) {
      axios
        .patch(`https://mate.academy/students-api/todos/${todo.id}?userId=10529`, todo.title, this.dataTodo)
        .then(() => {
          console.log('Todo updated successfully!');
        })
        .catch((error) => {
          console.error(error);
        });
    },

    removeAllCompleted() {
      const allCompleted = this.todos.filter(todo => todo.completed);
      allCompleted.forEach(todo => {
        axios.delete(`https://mate.academy/students-api/todos/${todo.id}?userId=10529`)
          .catch(error => console.log(error))
          .finally(() => {
            this.fetchTodos();
          })
      })
    },

    removeTodo(todoId) {
      axios
        .delete(`https://mate.academy/students-api/todos/${todoId}?userId=10529`)
        .catch(error => console.log(error))
        .finally(() => {
          this.fetchTodos();
        })
    },
    setFiltered(filter) {
      this.filtered = filter;
    },
  },

}
</script>
  
<style>
body {
  font-size: 18px;
}
  .todo {
    width: 600px;
    margin: auto;
    text-align: center;
  }
  
  .todo-container {
    box-shadow: 0 0 35px rgb(189, 192, 196);
    border: 2px solid rgb(175, 176, 179);
  }
  
  .todo-heading {
    font-size: 36px;
    text-align: center;
    padding-top: 100px;
    margin: auto;
    color: darkred;
    margin-bottom: 15px;
  }
  
  .todo-form {
    width: 100%;
    margin: auto;
  }
  
  .todo-input {
    width: 530px;
    height: 50px;
    border: transparent;
    font-size: 20px;
    padding-left: 20px;
    overflow: hidden;
  }
  
  .todo-list {
    list-style-type: none;
    padding: 0;
    width: 100%;
    margin: auto;
  }
  
  .todo-item {
    display: flex;
    align-items: center;
    border: 1px solid #ccc;
  }
  
  .todo-item-input {
    flex-grow: 1;
    padding: 8px;
    border: 1px transparent;
    height: 30px;
    font-size: 20px;
  }
  
  .todo-item-button {
    margin: 4px;
    margin-left: 8px;
    padding: 8px;
    background-color: #fff;
    color: darkred;
    border: transparent;
    cursor: pointer;
  }
  
  .done {
    text-decoration: line-through;
  }
  
  .todoapp__footer {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    border: 1px solid #ccc;
    height: 40px;
  }
  header {
    display: flex;
  }

  .header-button {
  margin: 8px;
  margin-top: 12px;
  background-color: transparent;
  color: #333;
  border: none;
  cursor: pointer;
  width: 45px;
  height: 25px;
  font-size: xx-large;
  transform:rotate(180deg);
  padding: 0;
  }

  .filter__link {
  margin-right: 10px;
  font-size: 16px;
  color: #333;
  text-decoration: none;
  padding: 5px 8px;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}
.filter {
  display: flex;
  align-items: center;
}

.filter__link.selected {
  background-color: darkred;
  color: #fff;
}
.todo-count {
  align-items: center;
  margin: auto 5px;
}

.todoapp__clear-completed {
  background-color: transparent;
  color: darkred;
  border: none;
  cursor: pointer;
  font-size: 16px;
  text-decoration: underline;
}
  </style>
  
