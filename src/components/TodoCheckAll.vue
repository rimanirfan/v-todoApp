<template>
    <div><label for=""><input type="checkbox" :checked="!anyRemaining" @change="allChecked">Check all</label></div>
</template>

<script>
export default {
    name: 'todo-check-all',
    computed: {
        anyRemaining() {
            return this.$store.getters.anyRemaining
        }
    },
    methods: {
        allChecked() {
            this.$store.state.todos.forEach((todo) => todo.completed = event.target.checked)
        }
    }
}
</script>

