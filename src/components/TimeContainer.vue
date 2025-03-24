<script setup>
// =================== Components ===================
import BPM from './BPM.vue';
import TimeSignature from './TimeSignature.vue';
</script>

<template>
    <!-- Contains the BPM and time signature components -->
    <div class="container">
        <BPM/>
        <TimeSignature/>
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

    border: solid black 3px;
}
</style>