<template>
    <div class="todo-item">
        <div class="todo-item-left">
            <input type="checkbox" name="" id="" v-model="todo.completed">
            <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed: todo.completed }">{{ todo.title }}</div>
            <input v-else class="todo-item-edit" type="text" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus v-model="todo.title">
        </div>
        <div class="remove-item" @click="removeTodo(index)">
            &times;
        </div>
    </div>
</template>

<script>
export default {
    name: 'todo-item',
    props: {
        todo: {
            type:Object,
            required:true,
        },
        index: {
            type:Number,
            required:true,
        }
    }
}
</script>
