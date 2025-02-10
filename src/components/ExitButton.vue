<script setup>
defineProps({
    Title: String
}) // end defineProps
</script>

<template>
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