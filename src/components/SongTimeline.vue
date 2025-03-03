<script setup>
import SongMeasure from './SongMeasure.vue';

// I imagine that a list of all the measures will need to be passed here
defineProps({
    Labels: Array
}) // end defineProps
</script>

<template>
    <div class="measureContainer">
        <SongMeasure v-for="label in Labels" :-measure-num="Labels.indexOf(label)" :-tick-num="label" />
    </div>
</template>

<style scoped>
.measureContainer {
    border: solid black 1px;
    width: 100%;

    display: flex;
    justify-content: left;
}
</style>

<script>
export default {

} // end export
</script>