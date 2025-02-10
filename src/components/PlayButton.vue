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

<script>
export default {
    methods: {
        play() {
            this.$emit("play");
        } // end play
    } // end methods
} // end export
</script>