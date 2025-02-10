<template>
    <button v-on:click="stop">STOP</button>
</template>

<style scoped>
    button {
        width: 32%;
        min-height: 25px;
    }
</style>

<script>
export default {
    methods: {
        stop() {
            this.$emit("stop");
        } // end stop
    } // end methods
} // end export
</script>