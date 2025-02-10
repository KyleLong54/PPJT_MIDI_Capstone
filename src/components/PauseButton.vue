<template>
    <button v-on:click="pause">PAUSE</button>
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
        pause() {
            this.$emit("pause");
        } // end pause
    } // end methods
} // end export
</script>