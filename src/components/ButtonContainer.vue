<script setup>
// =================== Libraries ===================
import { defineEmits } from 'vue';

// =================== Components ===================
import PlayButton from './PlayButton.vue';
import PauseButton from './PauseButton.vue';
import StopButton from './StopButton.vue';

// Setup Emits 
const emit = defineEmits(['play', 'pause', 'stop']);

// Tells the application to play playback
const play = () => {
    emit('play');
} // end play

// Tells the application to pause playback
const pause = () => {
    emit("pause");
} // end pause

// Tells the application to stop playback
const stop = () => {
    emit("stop");
} // end stop
</script>

<template>
    <!-- Contains play, pause, and stop buttons -->
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