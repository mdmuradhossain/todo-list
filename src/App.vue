<template>
  <div id="app">
    <Header/>
    <AddTodo v-on:add-todo="addTodo"/>

    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
    <h1 v-bind:name="name"></h1>
  </div>
</template>

<script>
import Todos from "./components/Todos";
import TodoItem from "./components/TodoItem";
import Header from "./components/layout/Header";
import AddTodo from "./components/AddTodo";
import axios from "axios";

export default {
  name: "app",
  components: {
    Todos,
    Header,
    AddTodo
  },
  data() {
    return {
      name: "Murad",
      todos: [
        // {
        //   id: 1,
        //   title: "Todo One",
        //   body: "This is todo one body",
        //   completed: false
        // },
        // {
        //   id: 2,
        //   title: "Todo Two",
        //   body: "This is todo two body",
        //   completed: false
        // },
        // {
        //   id: 3,
        //   title: "Todo Three",
        //   body: "This is todo three body",
        //   completed: false
        // }
      ]
    };
  },
  methods: {
    deleteTodo(id) {
      // this.todos = this.todos.filter(todo => {
      //   return todo.id != id;
      // });
      axios
        .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(res => {
          this.todos = this.todos.filter(todo => {
            return todo.id != id;
          });
        })
        .catch(err => {
          console.log(err);
        });
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;
      axios
        .post("https://jsonplaceholder.typicode.com/todos", {
          title,
          completed
        })
        .then(res => {
          return (this.todos = [...this.todos, res.data]);
        })
        .catch(err => {
          console.log(err);
        });
      // this.todos = [...this.todos, newTodo];
    }
  },
  created() {
    axios
      .get("https://jsonplaceholder.typicode.com/todos?_limit=10")
      .then(res => {
        return (this.todos = res.data);
      })
      .catch(err => {
        console.log(err);
      });
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
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover {
  background: #666;
}
</style>
