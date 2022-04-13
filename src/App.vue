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

      <v-spacer></v-spacer>

      <v-btn text>
        <span>More</span>
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <TodoHeader />
      <TodoList
        v-bind:propsdata="todoItems"
        @finishTodo="finishTodo"
        @removeTodo="removeTodo"
      ></TodoList>
    </v-main>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <!-- <v-footer color="primary">
      <TodoFooter v-on:removeAll="clearAll" />
    </v-footer> -->
  </v-app>
</template>

<script>
// import TodoFooter from "./components/TodoFooter.vue";
import TodoHeader from "./components/TodoHeader.vue";
import TodoList from "./components/TodoList.vue";
import TodoInput from "./components/TodoInput.vue";

export default {
  name: "App",

  components: {
    TodoList,
    // TodoFooter,
    TodoHeader,
    TodoInput,
  },

  data() {
    return {
      todoItems: [],
    };
  },
  methods: {
    clearAll() {
      localStorage.clear();
      this.todoItems = [];
    },
    addTodo(todoItem) {
      const { title, content } = todoItem;

      let todoObj = {};

      if (localStorage.getItem("todo") && JSON.parse(localStorage.getItem("todo")).length > 0) {
        const localTodo = JSON.parse(localStorage.getItem("todo"));
        todoObj.id = localTodo[localTodo.length - 1].id + 1;
      } else {
        todoObj.id = 1;
      }

      todoObj = { ...todoObj, state: "todo", title: title || "", content: content || "" };

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
    removeTodo(id) {
      this.todoItems = this.todoItems.filter((item) => item.id !== id);
      localStorage.setItem("todo", JSON.stringify(this.todoItems));
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
body {
  text-align: center;
  background-color: #f6f6f8;
}
.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>

