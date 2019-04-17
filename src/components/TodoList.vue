<template>
    <div>
        <todo-input
            v-model="todoText"
            placeholder="New Todos"
            @keydown.enter="addTodo"
        >
         </todo-input>
         <button v-if="todos.length" @click="toggleAll">{{ todos.every(todo => todo.done) ? 'uncheck all' : 'check all' }}</button>
         <ol class="todo-list">
             <todo-item
                v-for="todo in filterList"
                :key="todo.id"
                :todo="todo"
                @remove="removeTodo"
                @toggle="toggleStatus"
                @change="changeValue"
             >
             </todo-item>
         </ol>
         <todo-footer :filter="filter" @change="changeFilter"></todo-footer>
    </div>
</template>

<script>
import TodoInput from './TodoInput.vue';
import TodoItem from './TodoItem.vue';
import TodoFooter from './TodoFooter.vue';

export default {
    components: {
        'todo-input': TodoInput,
        'todo-item': TodoItem,
        'todo-footer': TodoFooter
    },
    watch: {
        todos: function (todos) {
            const id = todos.length ? todos[todos.length - 1].id + 1 : 0;
            localStorage.setItem('todos', JSON.stringify(todos));
            localStorage.setItem('id', id);
        }
    },
    data: function () {
        return {
            todoText: '',
            id: parseInt(localStorage.getItem('id')) || 0,
            todos: localStorage.getItem('todos') ? JSON.parse(localStorage.getItem('todos')) : [],
            filter: 'all'
        }
    },
    computed: {
        filterList: function () {
            if (this.filter === 'compeleted') {
                return this.todos.filter(todo => todo.done);
            } else if (this.filter === 'undone') {
                return this.todos.filter(todo => !todo.done);
            }
            return this.todos;
        }
    },
    methods: {
        addTodo: function() {
            const trimText = this.todoText.trim();
            if (!trimText) {
                return;
            }

            this.todos.push({
                id: this.id++,
                text: trimText,
                done: false
            });
            this.todoText = '';
        },
        removeTodo: function(id) {
            this.todos = this.todos.filter(todo => {
                return todo.id !== id
            })
        },
        toggleStatus: function(id) {
            this.todos = this.todos.map((todo) => {
                if (id === todo.id) {
                    return {
                        ...todo,
                        done: !todo.done
                    }
                }
                return todo;
            })
        },
        changeValue: function(todo) {
            const {id, text} = todo;
            this.todos = this.todos.map((todo) => {
                if (id === todo.id) {
                    return {
                        ...todo,
                        text
                    }
                }
                return todo;
            })
        },
        changeFilter: function (value) {
            this.filter = value;
        },
        toggleAll: function() {
            const isAllDone = this.todos.every(todo => todo.done);
            this.todos = this.todos.map(todo => ({
                ...todo,
                done: !isAllDone
            }))
        }
    }
}
</script>
