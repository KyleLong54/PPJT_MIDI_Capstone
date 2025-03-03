<script setup>
// =================== Setup Emits ===================
const emit = defineEmits(['pause']);

// =================== Functions ===================
const pause = () => {
    emit("pause");
} // end pause
</script>

<template>
    <button v-on:click="pause">PAUSE</button>
</template>

<style scoped>
button {
    width: 32%;
    min-height: 25px;
}
</style>