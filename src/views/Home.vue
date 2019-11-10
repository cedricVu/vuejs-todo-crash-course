<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" v-on:mark-complete="markComplete" />
  </div>
</template>

<script>
import Todos from '../components/Todos.vue';
import AddTodo from '../components/layouts/AddTodo.vue';
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      axios
      .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(() => this.todos = this.todos.filter(todo => todo.id !== id))
      .catch(e => console.log(e));
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;
      axios
      .post('https://jsonplaceholder.typicode.com/todos', {
        title,
        completed
      })
      .then(res => this.todos = [...this.todos, res.data])
      .catch(e => console.log(e));
    },
    markComplete(id) {
      axios
      .put(`https://jsonplaceholder.typicode.com/todos/${id}`, {
        completed: true
      })
      .then(() => this.todos = this.todos.filter(item => {
        if (item.id === id) {
          item.completed = true;
        }
        return true;
      }))
      .catch(e => console.log(e));
    }
  },
  created() {
    axios
    .get('https://jsonplaceholder.typicode.com/todos?_limit=5')
    .then(res => {
      this.todos = res.data;
    })
    .catch(e => console.log(e));
  }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4
  }
  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }
</style>