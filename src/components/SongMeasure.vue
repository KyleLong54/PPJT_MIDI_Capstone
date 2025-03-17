<script setup>
// =================== Define Properties ===================
defineProps({
    MeasureNum: Number,
    TickNum: Number,
}) // end defineProps
</script>

<template>
    <div class="outline">
        <!-- <p style="font-size: 9px;">{{ MeasureNum }}</p> -->
        <button v-on:click="$emit('skip', MeasureNum)"><img src="../assets/Black Dot.png" alt="Measure Dot"></button>
    </div>
</template>

<style scoped>
.outline {
    border: solid black 1px;
    min-width: 35px;
    min-height: 35px;

    display: flex;
    justify-content: center;
    align-items: center;
}

button {
    width: 30px;
    height: 30px;

    border: 0px;

    display: flex;
    justify-content: center;
    align-items: center;
}


img {
    height: 26px;
    width: 26px;
}

img:hover {
    cursor: pointer;
}

.outline:hover {
    background-color: lightgray;
}

.outline:hover button {
    background-color: lightgray;
}
</style>