<script setup>
// =================== Setup Emits ===================
const emit = defineEmits(['stop']);

// =================== Functions ===================
const stop = () => {
    emit('stop');
} // end stop
</script>

<template>
    <button v-on:click="stop">STOP</button>
</template>

<style scoped>
    button {
        width: 32%;
        min-height: 25px;
    }
</style>