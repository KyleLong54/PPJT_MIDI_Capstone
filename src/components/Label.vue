<script setup>
// Define properties
defineProps({
    Title: String,
    MeasureNum: Number,
}) // end defineProps
</script>

<template>
    <!-- Label in a song, linked to a measure -->
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