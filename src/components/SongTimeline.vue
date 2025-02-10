<script setup>
import SongMeasure from './SongMeasure.vue';

// I imagine that a list of all the measures will need to be passed here
defineProps({
    
}) // end defineProps
</script>

<template>
    <div class="measureContainer">
        <SongMeasure -measure-num="1" -tick-num="0"/>
        <SongMeasure -measure-num="2" -tick-num="420"/>
        <SongMeasure -measure-num="3" -tick-num="840"/>
        <SongMeasure -measure-num="4" -tick-num="1260"/>
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