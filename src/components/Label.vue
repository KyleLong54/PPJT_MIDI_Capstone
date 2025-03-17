<script setup>
defineProps({
    Title: String,
    MeasureNum: Number,
}) // end defineProps
</script>

<template>
    <a v-on:click="$emit('skip', MeasureNum)">{{ Title }}</a>
</template>

<style scoped>
a {
    color: red;

    width: 100%;
}

a:hover {
    cursor: pointer;
}
</style>