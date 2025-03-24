<script setup>
// Setup Emits
const emit = defineEmits(['stop']);

// Tells the application to stop the playback
const stop = () => {
    emit('stop');
} // end stop
</script>

<template>
    <!-- Stop button -->
    <button v-on:click="stop">STOP</button>
</template>

<style scoped>
    button {
        width: 32%;
        min-height: 25px;
    }
</style>