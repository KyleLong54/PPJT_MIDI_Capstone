<script setup>
// =================== Libraries ===================
import { defineEmits } from 'vue';

// =================== Components ===================
import PlayButton from './PlayButton.vue';
import PauseButton from './PauseButton.vue';
import StopButton from './StopButton.vue';

// =================== Setup Emits ===================
const emit = defineEmits(['play', 'pause', 'stop']);

// =================== Fuctions ===================
const play = () => {
    emit('play');
} // end play

const pause = () => {
    emit("pause");
} // end pause

const stop = () => {
    emit("stop");
} // end stop
</script>

<template>
    <div class="container">
        <PlayButton @play="play" />
        <PauseButton @pause="pause" />
        <StopButton @stop="stop" />
    </div>
</template>

<style scoped>
.container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;

    gap: 5px;

    width: 32%;
    height: 100%;

    padding: 1%;
}
</style>