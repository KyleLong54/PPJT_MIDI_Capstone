<script setup>
import { defineEmits } from 'vue';

const emit = defineEmits(['play'])

const play = () => {
    emit('play');
} // end play
</script>

<template>
    <button v-on:click="play">PLAY</button>
</template>

<style scoped>
button {
    width: 32%;
    height: 100%;
    min-height: 25px;
}
</style>