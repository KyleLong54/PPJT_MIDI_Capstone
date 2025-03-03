<script setup>
defineProps({
    Title: String,
    MeasureNum: Number,
}) // end defineProps
</script>

<template>
    <a v-on:click="test()">{{ Title }}</a>
</template>

<style scoped>
a {
    color: red;

    width: 100%;
}

a:hover {
    cursor: pointer;
}
</style>

<script>
export default {
methods: {
    test() {
        alert(this.MeasureNum)
    }
}
} // end export
</script>