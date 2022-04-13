<template>
    <div class="inputBox shadow">
        <v-row no-gutters align="center" class="pa-4">
            <v-text-field
                label="오늘은 무슨 일정이 있으신가요?"
                single-line
                outlined
                autofocus
                rounded
                hide-details
                v-model="newTodoItem"
                v-on:keyup.enter="addTodoTitle"
            ></v-text-field>

            <!-- 제목 + 내용 입력할 수 있는 UI를 띄워주는 버튼: 입력값이 없을 때 보임 -->
            <v-btn
                v-if="!newTodoItem"
                class="mx-2"
                fab
                dark
                color="deep-orange"
            >
                <v-icon dark>
                    mdi-plus
                </v-icon>
            </v-btn>

            <!-- 제목만 간단하게 제출하는 버튼: 입력값이 있을 때 보임 -->
            <v-btn
                v-else
                class="mx-2"
                fab
                dark
                v-on:click="addTodoTitle"
                color="indigo"
            >
                <v-icon dark>
                    mdi-check
                </v-icon>
            </v-btn>
        </v-row>

        <modal v-if="showModal" @close="showModal = false">
            <h3 slot="header">경고</h3>
            <span slot="footer" @click="showModal = false">할 일을 입력하세요.
                <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
            </span>
        </modal>
    </div>
</template>

<script>
import Modal from './common/AlertModal.vue'

export default {
    data() {
        return {
            newTodoItem: '',
            showModal: false
        }
    },
    methods: {
        addTodoTitle() {
            if (this.newTodoItem !== "") {
                var title = this.newTodoItem && this.newTodoItem.trim();
                this.$emit('addTodo', { title })
                this.clearInput();
            } else {
                this.showModal = !this.showModal;
            }
        },
        clearInput() {
            this.newTodoItem = '';
        }
    },
    components: {
        Modal: Modal
    }
}
</script>

<style scoped>
</style>
