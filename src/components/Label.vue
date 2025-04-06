<script setup>
// Define properties
defineProps({
    Title: String,
    MeasureNum: Number,
}) // end defineProps
</script>

<template>
    <!-- Label in a song, linked to a measure -->
    <a v-on:click="$emit('skip', MeasureNum - 1)">{{ Title }}</a>
</template>

<style scoped>
a {
    color: var(--color-accent-ppjt-green);

    padding-left: 3px;

    width: 100%;
}

a:hover {
    cursor: pointer;
}
</style>