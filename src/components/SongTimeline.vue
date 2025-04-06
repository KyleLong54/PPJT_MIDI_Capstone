<script setup>
// =================== Components ===================
import SongMeasure from './SongMeasure.vue';

// I imagine that a list of all the measures will need to be passed here
defineProps({
    Measures: Array
}) // end defineProps

// Define Emits
const emit = defineEmits(['skip']);

// Tells the application to skip to a given measure
const skip = (measureNum) => {
    emit('skip', measureNum);
} // end skip
</script>

<template>
    <!-- Displays a song measure component for each measure in the song -->
    <div class="measureContainer">
        <SongMeasure v-for="(measure, index) in Measures" :-measure-num="index" :-tick-num="measure" @skip="skip" />
    </div>
</template>

<style scoped>
.measureContainer {
    display: flex;
    justify-content: left;

    gap: 1px;

    padding-top: 1px;
    padding-bottom: 1px;
    padding-right: 2px;
    padding-left: 2px;
}

/* Colors */
.measureContainer {
    background-color: var(--color-accent-ppjt-green);

    border: 1px solid var(--color-border);
    border-radius: 8px;
}
</style>