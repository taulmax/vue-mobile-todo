<template>
  <v-container>
    <transition-group class="pl-0" name="list" tag="ul">
      <v-card class="mb-2 " v-for="(todoItem) in propsdata" :key="todoItem.id">
        <v-card-actions>
          <v-list-item>
            <v-list-item-avatar>
              <v-icon class="grey lighten-1" dark> mdi-check </v-icon>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title v-text="todoItem.title"></v-list-item-title>
            </v-list-item-content>

            <v-list-item-action @click="removeTodo(todoItem.id)">
              <v-btn icon>
                <v-icon>mdi-close</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-card-actions>
      </v-card>
    </transition-group>
  </v-container>
</template>

<script>
export default {
  props: ["propsdata"],
  methods: {
    removeTodo(id) {
      this.$emit("removeTodo", id);
    },
  },
};
</script>

<style scoped>
ul {
  list-style-type: none;
  padding-left: 0px;
  margin-top: 0;
  text-align: left;
}
li {
  display: flex;
  min-height: 50px;
  height: 50px;
  line-height: 50px;
  margin: 0.5rem 0;
  padding: 0 0.9rem;
  background: white;
  border-radius: 5px;
}
.checkBtn {
  line-height: 45px;
  color: #62acde;
  margin-right: 5px;
}
.removeBtn {
  margin-left: auto;
  color: #de4343;
}

.list-enter-active,
.list-leave-active {
  transition: all 1s;
}
.list-enter,
.list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style>
