<script setup>
// Define Properties
defineProps({
    MeasureNum: Number,
    TickNum: Number,
}) // end defineProps
</script>

<template>
    <div class="outline">
        <!-- <p style="font-size: 9px;">{{ MeasureNum }}</p> -->

        <!-- Button with black circle -->
        <button v-on:click="$emit('skip', MeasureNum)"><img src="../assets/Black Dot.png" alt="Measure Dot"></button>
        <!-- NOTE: The button also tells the application to skip to the associated measure -->
    </div>
</template>

<style scoped>
.outline {
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

/* Colors */
img:hover {
    cursor: pointer;
}

button {
    background-color: var(--color-bg-panel);
}

.outline {
    background-color: var(--color-bg-panel);

    border: 1px solid var(--color-button-border);
    border-radius: 5px;

    transition: background-color 0.2s ease;
}

.outline:hover {
    background-color: var(--color-accent-ppjt-green-light);
    border: 1px solid transparent;
}

.outline:hover button {
    background-color: var(--color-accent-ppjt-green-light);
    border: 1px solid transparent;
}
</style>