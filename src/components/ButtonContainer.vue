<script setup>
    import PlayButton from './PlayButton.vue';
    import PauseButton from './PauseButton.vue';
    import StopButton from './StopButton.vue';
</script>

<template>
    <div class="container">
        <PlayButton @play="play"/>
        <PauseButton @pause="pause"/>
        <StopButton @stop="stop"/>
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

<script>
export default {
    methods: {
        play() {
            this.$emit("play");
        }, // end play

        pause() {
            this.$emit("pause");
        }, // end pause

        stop() {
            this.$emit("stop");
        }, // end stop
    } // end methods
} // end export
</script>