<script setup>
    import PlayButton from './PlayButton.vue';
    import PauseButton from './PauseButton.vue';
    import StopButton from './StopButton.vue';
</script>

<template>
    <div class="container">
        <PlayButton/>
        <PauseButton/>
        <StopButton/>
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

    border: solid 2px purple;
}
</style>