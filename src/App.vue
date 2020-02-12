<template>
  <div id="app">
    <Header />
    <AddToDo v-on:add-todo="addToDo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteToDo" />
  </div>
</template>

<script>
import Header from "./components/layout/Header";
import Todos from "./components/Todos";
import AddToDo from "./components/AddToDo";
import axios from 'axios';
export default {
  name: "App",
  components: {
    Todos,
    Header,
    AddToDo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    deleteToDo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`).
      then(() => this.todos = this.todos.filter(todo => todo.id !== id)).
      catch(e => console.log(e));

    },
    addToDo(newToDo) {
      const { title, completed } = newToDo;

      axios.post('https://jsonplaceholder.typicode.com/todos', {
        title,
        completed
      }).
      then(res => this.todos = [...this.todos, res.data]).
      catch(e => console.log(e));
    }
  },
  created() {
    axios.get("https://jsonplaceholder.typicode.com/todos?_limit=5")
      .then(res => this.todos = res.data)
      .catch(e => console.log(e));
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #ffffff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover {
  background: #666;
}
</style>
