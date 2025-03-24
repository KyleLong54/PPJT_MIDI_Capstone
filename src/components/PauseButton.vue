<script setup>
// Setup Emits 
const emit = defineEmits(['pause']);

// Tells application to pause playback
const pause = () => {
    emit("pause");
} // end pause
</script>

<template>
    <!-- Button to pause playback -->
    <button v-on:click="pause">PAUSE</button>
</template>

<style scoped>
button {
    width: 32%;
    min-height: 25px;
}
</style>