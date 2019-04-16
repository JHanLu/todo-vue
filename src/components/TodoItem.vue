<template>
    <li>
        {{ todo.text }}
        <button @click="$emit('remove', todo.id)">
            &times;
        </button>
    </li>
</template>

<script>
export default {
    props: ['todo'],
}
</script>
