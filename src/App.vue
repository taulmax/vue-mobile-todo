<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />
      </div>
    </v-app-bar>

    <v-main>
      <div class="main_wrapper">
        <TodoHeader v-bind:propsdata="todoItems" />
        <TodoList
          v-bind:propsdata="todoItems"
          @finishTodo="finishTodo"
          @updateTodo="updateTodo"
          @removeTodo="removeTodo"
        ></TodoList>
        <TodoFooter v-if="getTodoItemsLength() > 0" v-on:removeAll="clearAll" />
      </div>
    </v-main>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
  </v-app>
</template>

<script>
import TodoFooter from "./components/TodoFooter.vue";
import TodoHeader from "./components/TodoHeader.vue";
import TodoList from "./components/TodoList.vue";
import TodoInput from "./components/TodoInput.vue";

export default {
  name: "App",

  components: {
    TodoList,
    TodoFooter,
    TodoHeader,
    TodoInput,
  },

  data() {
    return {
      todoItems: [],
    };
  },
  methods: {
    getTodoItemsLength() {
      return this.todoItems.filter((item) => item.state === "todo").length;
    },
    addTodo(todoItem) {
      const { title, memo } = todoItem;

      let todoObj = {};

      if (localStorage.getItem("todo") && JSON.parse(localStorage.getItem("todo")).length > 0) {
        const localTodo = JSON.parse(localStorage.getItem("todo"));
        todoObj.id = localTodo[localTodo.length - 1].id + 1;
      } else {
        todoObj.id = 1;
      }

      todoObj = { ...todoObj, state: "todo", title: title || "", memo: memo || "" };

      this.todoItems.push(todoObj);
      localStorage.setItem("todo", JSON.stringify(this.todoItems));
    },
    finishTodo(id) {
      this.todoItems = this.todoItems.map((item) => {
        if (item.id === id) {
          return { ...item, state: "done" };
        } else {
          return item;
        }
      });
      localStorage.setItem("todo", JSON.stringify(this.todoItems));
    },
    updateTodo(todo) {
      const updatedTodoIndex = this.todoItems.findIndex((item) => item.id === todo.id);
      this.todoItems[updatedTodoIndex] = { ...todo, state: "todo" };
      localStorage.setItem("todo", JSON.stringify(this.todoItems));
    },
    removeTodo(id) {
      this.todoItems = this.todoItems.filter((item) => item.id !== id);
      localStorage.setItem("todo", JSON.stringify(this.todoItems));
    },
    clearAll() {
      localStorage.clear();
      this.todoItems = [];
    },
  },
  created() {
    if (localStorage.getItem("todo")) {
      this.todoItems = JSON.parse(localStorage.getItem("todo"));
    }
  },
};
</script>

<style>
html {
  height: 100vh;
  overflow: hidden !important;
}
body {
  height: 100%;
  text-align: center;
  background-color: #f6f6f8;
}
#app {
  height: 100%;
}
.v-main {
  height: 100%;
  padding: 0 !important;
  margin: 56px 0px 88px 0px !important; 
}
.main_wrapper {
  height: calc(100% - 56px - 88px);
  overflow-y: auto;
  overflow-x: hidden;
}
.shadow {
  -webkit-box-shadow: 0px 0px 5px 0px rgba(181,181,181,1);
  -moz-box-shadow: 0px 0px 5px 0px rgba(181,181,181,1);
  box-shadow: 0px 0px 5px 0px rgba(181,181,181,1);
}
</style>

