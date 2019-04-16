<template>
    <div>
        <todo-input
            v-model="todoText"
            placeholder="New Todos"
            @keydown.enter="addTodo"
        >
         </todo-input>
         <ol class="todo-list">
             <todo-item
                v-for="todo in todos"
                :key="todo.id"
                :todo="todo"
                @remove="removeTodo"
             >
             </todo-item>
         </ol>
    </div>
</template>

<script>
import TodoInput from './TodoInput.vue';
import TodoItem from './TodoItem.vue';

export default {
    components: {
        'todo-input': TodoInput,
        'todo-item': TodoItem
    },
    data: function () {
        return {
            todoText: '',
            id: 0,
            todos: [],
            filter: 'all'
        }
    },
    methods: {
        addTodo: function() {
            const trimText = this.todoText.trim();

            this.todos.push({
                id: this.id++,
                text: trimText
            });
            this.todoText = '';
        },
        removeTodo: function(id) {
            this.todos = this.todos.filter(todo => {
                return todo.id !== id
            })
        }
    }
}
</script>
