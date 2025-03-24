<script setup>
// Define properties
defineProps({
    Title: String
}) // end defineProps
</script>

<template>
    <!-- Exit button, to exit back to homepage -->
    <button v-on:click="exit">Exit {{ Title }}</button>
</template>

<style scoped>
</style>

<script>
export default {
    methods: {
        exit() {
            this.$emit("exit")
        }
    }, // end methods
} // end export
</script>