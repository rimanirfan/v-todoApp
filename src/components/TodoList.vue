<template>
    <div>
        <input 
            type="text" 
            class="todo-input" 
            placeholder="What needs to be done?" 
            v-model="newTodo"
            @keyup.enter="addTodo">
        <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
        <todo-item v-for="(todo, index) in todosFiltered" :key="todo.id" :todo="todo" :index="index" :checkAll="!anyRemaining">
        </todo-item>
        </transition-group>
        <div class="extra-container">
            <todo-check-all></todo-check-all>
            <todo-items-remaining></todo-items-remaining>
        </div>

        <div class="extra-container">
            <todo-filter></todo-filter>
            <div>
                <transition name="fade">
                    <todo-completed></todo-completed>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
import TodoItem from './TodoItem'
import TodoItemsRemaining from './TodoItemsRemaining'
import TodoCheckAll from './TodoCheckAll'
import TodoFilter from './TodoFilter'
import TodoCompleted from './TodoCompleted'


export default {
    components: {
        TodoItem,
        TodoItemsRemaining,
        TodoCheckAll,
        TodoFilter,
        TodoCompleted
    },
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
    created() {
        // eventBus.$on('removedTodo', (index) => this.removeTodo(index))
        // eventBus.$on('finishedEdit', (data) => this.finishedEdit(data))
        // eventBus.$on('checkAllChanged', (checked) => this.checkAllTodos(checked))
        // eventBus.$on('filterChanged', (filter) => this.$store.state.filter = filter)
        // eventBus.$on('clearCompletedTodos', () => this.clearCompleted())
    },
    beforeDestroy() {
        // eventBus.$off('removedTodo', (index) => this.removeTodo(index))
        // eventBus.$off('finishedEdit', (data) => this.finishedEdit(data))
        // eventBus.$off('checkAllChanged', (checked) => this.checkAllTodos(checked))
        // eventBus.$off('filterChanged', (filter) => this.$store.state.filter = filter)
        // eventBus.$off('clearCompletedTodos', () => this.clearCompleted())
    },
    computed: {
        // remaining() {
        //     return this.$store.getters.remaining
        // },
        anyRemaining() {
            return this.$store.getters.anyRemaining
        },
        todosFiltered() {
            return this.$store.getters.todosFiltered
        },
        // showClearCompletedBtn() {
        //     return this.$store.getters.showClearCompletedBtn
        // },
    },
    methods: {
        addTodo() {
            if(this.newTodo.trim().length == 0) {
                return
            }

            this.$store.state.todos.push({
                id: this.idTodo,
                title: this.newTodo,
                completed: false,
            })

            this.newTodo = ''
            this.idTodo++
        },
        // removeTodo(index) {
        //     this.$store.state.todos.splice(index, 1)
        // },
        // checkAllTodos() {
        //     this.$store.state.todos.forEach((todo) => todo.completed = event.target.checked)
        // },
        // clearCompleted() {
        //     this.$store.state.todos = this.$store.state.todos.filter(todo => !todo.completed)
        // },
        // finishedEdit(data) {
        //     const index = this.$store.state.todos.findIndex(item => item.id == data.id)
        //     this.$store.state.todos.splice(data.index, 1, data.todo)
        // }
    },
}
</script>

<style lang="scss">
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

