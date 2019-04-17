<template>
    <li>
        <button @click="$emit('toggle', todo.id)">{{ todo.done ? 'undo' : 'done' }}</button>
        <input v-focus v-if="editing" :class="{ slash: todo.done }" @blur="quitEddingting" type="text" v-model="temp" />
        <span v-else :class="{ slash: todo.done }" @dblclick="toggleEditing">{{ todo.text }}</span>
        <button @click="$emit('remove', todo.id)">
            &times;
        </button>
    </li>
</template>

<script>
export default {
    props: ['todo'],
    data: function() {
        return {
            editing: false,
            temp: this.todo.text
        }
    },
    methods: {
        toggleEditing: function($event) {
            this.editing = true;
        },
        quitEddingting: function() {
            this.editing = false;
            if (!this.temp) {
                this.temp = this.todo.text;
            } else {
                this.$emit('change', {
                    id: this.todo.id,
                    text: this.temp
                })
            }
        }
    },
    directives: {
        focus: {
            inserted: function(el) {
                el.focus();
            }
        }
    }
}
</script>

<style>
    .slash {
        text-decoration: line-through;
    }
</style>
