<template>
    <div>
        <input 
            type="text" 
            class="todo-input" 
            placeholder="What needs to be done?" 
            v-model="newTodo"
            @keyup.enter="addTodo">
        <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
        <div class="todo-item" v-for="(todo, index) in todosFiltered" :key="todo.id">
            <div class="todo-item-left">
                <input type="checkbox" name="" id="" v-model="todo.completed">
                <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed: todo.completed }">{{ todo.title }}</div>
                <input v-else class="todo-item-edit" type="text" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus v-model="todo.title">
            </div>
            <div class="remove-item" @click="removeTodo(index)">
                &times;
            </div>
        </div>
        </transition-group>
        <div class="extra-container">
            <div><label for=""><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Check all</label></div>
            <div>{{ remaining }} items left</div>
        </div>

        <div class="extra-container">
            <div>
                <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
                <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button>
                <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
            </div>

            <div>
                <transition name="fade">
                    <button v-if="showClearCompletedBtn" @click="clearCompleted">Clear Completed</button>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            newTodo: '',
            beforeEditTitle: '',
            idTodo: 3,
            filter: 'all',
            todos: [
                {
                    'id': 1,
                    'title': 'Finish Vue Screentest',
                    'completed': false,
                    'editing': false,
                },
                {
                    'id': 2,
                    'title': 'Take over world',
                    'completed': false,
                    'editing': false,
                }
            ],
        };
    },
    directives: {
        focus: {
            inserted: function (el) {
                el.focus()
            }
        }
    },
    computed: {
        remaining() {
            return this.todos.filter(todo => !todo.completed).length
        },
        anyRemaining() {
            return this.remaining != 0
        },
        todosFiltered() {
            if (this.filter == 'all'){
                return this.todos
            } else if (this.filter == 'active') {
                return this.todos.filter(todo => !todo.completed)
            } else if (this.filter == 'completed') {
                return this.todos.filter(todo => todo.completed)
            }
        },
        showClearCompletedBtn() {
            return this.todos.filter(todo => todo.completed).length > 0
        },
    },
    methods: {
        addTodo() {
            if(this.newTodo.trim().length == 0) {
                return
            }

            this.todos.push({
                id: this.idTodo,
                title: this.newTodo,
                completed: false,
            })

            this.newTodo = ''
            this.idTodo++
        },
        editTodo(todo) {
            this.beforeEditTitle = todo.title
            todo.editing = true
        },
        doneEdit(todo) {
            if(todo.title.trim().length == 0) {
                todo.title = this.beforeEditTitle
            }

            todo.editing = false
        },
        cancelEdit(todo) {
            todo.title = this.beforeEditTitle
            todo.editing = false;
        },
        removeTodo(index) {
            this.todos.splice(index, 1)
        },
        checkAllTodos() {
            this.todos.forEach((todo) => todo.completed = event.target.checked)
        },
        clearCompleted() {
            this.todos = this.todos.filter(todo => !todo.completed)
        }
    },
}
</script>

<style lang="scss" scoped>
    @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.0/animate.min.css");

    .todo-input {
        width: 100%;
        padding: 10px 18px;
        margin-bottom: 16px;
        font-size: 16px;
    }

    .todo-item {
        margin-bottom: 15px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: .3;
    }

    .todo-item-left {
        display: flex;
        align-items: center;
    }

    .todo-item-label {
        padding: 10px;
        border: 1px solid #fff;
        margin-left: 12px;
    }

    .todo-item-edit {
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Arial, Helvetica, sans-serif;

        &:focus {
            outline: none;
        }
    }

    .remove-item {
        cursor: pointer;
        margin-left: 15px;

        &:hover {
            color:black;
        }
    }

    .completed {
        color:grey;
        text-decoration: line-through;
    }

    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
    }

    button {
        font-size: 14px;
        background-color: #fff;
        appearance: none;

        &:hover {
            background: lightgreen;
        }

        &:focus {
            outline: none;
        }
    }

    .active {
        background: lightgreen;
    }

    //CSS Transition
    .fade-enter-active, .fade-leave-active {
        transition: opacity .2s;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0;
    }
</style>

