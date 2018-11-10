<template>
    <button v-if="showClearCompletedBtn" @click="clearCompleted">Clear Completed</button>
</template>

<script>
export default {
    name: 'todo-completed',
    props: {
        showClearCompletedBtn: {
            type: Boolean,
            required: true
        }
    },
    methods: {
        clearCompleted() {
            eventBus.$emit('clearCompletedTodos')
        }
    }
}
</script>
