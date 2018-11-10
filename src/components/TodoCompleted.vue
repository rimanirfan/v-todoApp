<template>
    <button v-if="showClearCompletedBtn" @click="clearCompleted">Clear Completed</button>
</template>

<script>
export default {
    name: 'todo-completed',
    computed: {
        showClearCompletedBtn() {
            return this.$store.getters.showClearCompletedBtn
        }
    },
    methods: {
        clearCompleted() {
            this.$store.state.todos = this.$store.state.todos.filter(todo => !todo.completed)
        }
    }
}
</script>
