<template>
  <v-container>
    <transition-group class="pl-0" name="list" tag="ul">
      <v-card class="mb-2 " v-for="(todoItem) in propsdata" :key="todoItem.id" v-show="(todoItem.state === 'todo')">
        <v-card-actions>
          <v-list-item>
            <v-list-item-avatar @click="finishTodo(todoItem.id)">
              <v-icon class="grey lighten-1" dark> mdi-check </v-icon>
            </v-list-item-avatar>

            <v-list-item-content @click="showTodoDetail(todoItem.id)">
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

    <v-dialog
      v-model="showDialog"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
    >
        <v-card>
            <v-toolbar
                dark
                color="primary"
            >
                <v-btn
                    icon
                    dark
                    @click="closeTodoDetail()"
                >
                    <v-icon>mdi-close</v-icon>
                </v-btn>
                <v-toolbar-title>Detail</v-toolbar-title>
            </v-toolbar>
            <v-list>
                <v-list-item>
                    <v-text-field
                        label="할 일"
                        hide-details
                        outlined
                        :readonly="updateMode ? false : true"
                        v-model="selectedTodoTitle"
                        @click="openUpdateMode()"
                        class="pa-4"
                    ></v-text-field>
                </v-list-item>
                <v-list-item>
                    <v-textarea
                        label="메모"
                        hide-details
                        outlined
                        :readonly="updateMode ? false : true"
                        v-model="selectedTodoMemo"
                        @click="openUpdateMode()"
                        no-resize
                        class="pa-4"
                    ></v-textarea>
                </v-list-item>
            </v-list>
            <v-bottom-navigation fixed>
                <v-row no-gutters v-if="!updateMode">
                    <v-col>
                        <v-btn block height="100%" @click="clickFinish()">
                            <span>완료</span>
                        </v-btn>
                    </v-col>
                    <v-col>
                        <v-btn block height="100%" @click="clickEdit()">
                            <span>편집</span>
                        </v-btn>
                    </v-col>
                    <v-col>
                        <v-btn block height="100%" @click="clickDelete()">
                            <span>삭제</span>
                        </v-btn>
                    </v-col>
                </v-row>
                <v-row no-gutters v-else>
                    <v-col>
                        <v-btn block height="100%" @click="clickCancel()">
                            <span>취소</span>
                        </v-btn>
                    </v-col>
                    <v-col>
                        <v-btn block height="100%" @click="clickSave()">
                            <span>저장</span>
                        </v-btn>
                    </v-col>
                </v-row>
            </v-bottom-navigation>
        </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  props: ["propsdata"],
  data() {
    return {
      selectedTodo: null,
      selectedTodoTitle: "",
      selectedTodoMemo: "",
      showDialog: false,
      updateMode: false,
    }
  },
  methods: {
    finishTodo(id) {
      this.$emit("finishTodo", id);
    },
    removeTodo(id) {
      this.$emit("removeTodo", id);
    },
    showTodoDetail(id) {
      const selectedTodo = this.propsdata.find((item) => item.id === id);
      this.selectedTodo = selectedTodo;
      this.selectedTodoTitle = selectedTodo.title;
      this.selectedTodoMemo = selectedTodo.memo;
      this.showDialog = true;
    },
    closeTodoDetail() {
      this.selectedTodo = null;
      this.selectedTodoTitle = "";
      this.selectedTodoMemo = "";
      this.showDialog = false;
      this.updateMode = false;
    },
    openUpdateMode() {
      if (!this.updateMode) {
        this.updateMode = true;
      }
    },
    clickFinish() {
      this.finishTodo(this.selectedTodo.id);
      this.showDialog = false;
    },
    clickEdit() {
      this.updateMode = true;
    },
    clickDelete() {
      this.removeTodo(this.selectedTodo.id);
      this.showDialog = false;
    },
    clickCancel() {
      const { title, memo } = this.selectedTodo;
      this.selectedTodoTitle = title;
      this.selectedTodoMemo = memo;
      this.updateMode = false;
    },
    clickSave() {
      this.$emit("updateTodo", { id: this.selectedTodo.id, title: this.selectedTodoTitle, memo: this.selectedTodoMemo });
      this.updateMode = false;
    }
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
