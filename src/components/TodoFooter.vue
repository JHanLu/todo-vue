<template>
    <div class="filter-list">
        <span 
            v-for="(li, index) in filterList" 
            :key="index" 
            :class="{ active: li === filter }"
            @click="$emit('change', li)"
        >
            {{ li }}
        </span>
    </div>
</template>

<script>
export default {
    props: ['filter'],
    data: function() {
        return {
            filterList: ['all', 'compeleted', 'undone']
        }
    }
}
</script>

<style>
    .filter-list span {
        margin: 0 5px;
        cursor: pointer;
    }

    .filter-list span.active {
        border: 1px solid;
    }
</style>

