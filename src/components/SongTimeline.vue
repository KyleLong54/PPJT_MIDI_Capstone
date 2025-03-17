<script setup>
import SongMeasure from './SongMeasure.vue';

// I imagine that a list of all the measures will need to be passed here
defineProps({
    Measures: Array
}) // end defineProps

const emit = defineEmits(['skip']);

const skip = (measureNum) => {
    emit('skip', measureNum);
} // end skip
</script>

<template>
    <div class="measureContainer">
        <SongMeasure v-for="(measure, index) in Measures" :-measure-num="index" :-tick-num="measure" @skip="skip" />
    </div>
</template>

<style scoped>
.measureContainer {
    border: solid black 1px;

    display: flex;
    justify-content: left;
}
</style>