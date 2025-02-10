<script setup>
defineProps({
    MeasureNum: Number,
    TickNum: Number,
}) // end defineProps
</script>

<template>
    <div class="outline">
        <button style="width: 30px; height: 30px;">O</button>
    </div>
</template>

<style scoped>
.outline {
    border: solid black 1px;
    width: 35px;
    height: 35px;

    display: flex;
    justify-content: center;
    align-items: center;
}

button {
    border: solid black 1px;
}
</style>

<script>
export default {

} // end export
</script>